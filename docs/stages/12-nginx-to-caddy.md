# Stage 12 — Replace nginx with Caddy in all frontend images

**Repos:** schlussel, schloss, kuvert (three separate branches/PRs) · **Status:** done

Merged: [schlussel#25](https://github.com/zudaR107/schlussel/pull/25) ·
[schloss#23](https://github.com/zudaR107/schloss/pull/23) ·
[kuvert#32](https://github.com/zudaR107/kuvert/pull/32)

Each repo's runner stage now uses `caddy:2-alpine`; static files copied to
`/srv` instead of `/usr/share/nginx/html`. kuvert's `/api/*` route needed
`handle_path` (not plain `handle`) to replicate nginx's trailing-slash
`proxy_pass` prefix-stripping, since kuvert-api's routes are mounted at
root rather than under `/api`. schlussel/web and schloss's `/auth/*`
proxies didn't need stripping (nginx forwarded the full path there too),
so those stayed as plain `handle` + `reverse_proxy`.

Verified with real multi-container runs per repo (not just config
review): register/login end-to-end through each Caddy proxy, cross-service
JWKS token validation via kuvert-api, and SPA routing on `/auth/callback`
confirmed served locally rather than proxied — the exact class of bug this
migration was meant to prevent.

## Why

All three static frontends (schlussel/web, schloss, kuvert/web) ship as
`nginx:alpine` + a hand-written `nginx.conf` doing SPA fallback plus
reverse-proxying `/auth/*` (and `/api/*` for kuvert) to sibling containers.
This has already caused one real bug twice over (Stage 4 and Stage 5):
nginx's prefix-location matching picks the *longest* matching prefix
regardless of where it's declared in the file, so a client-side SPA route
under `/auth/callback` silently got swallowed by the more general `/auth/`
reverse-proxy block and 404'd — fixed both times by adding a second,
longer, more-specific `location` block ahead of the proxy one.

For a self-hosted pet project with no ops team, Caddy is a better fit:
- `handle` blocks are evaluated top-to-bottom, first match wins — no
  implicit longest-prefix surprise like nginx's `location` matching.
- Reverse proxy config is a one-liner (`reverse_proxy schlussel:4000`) vs
  nginx's `proxy_pass` + three `proxy_set_header` lines repeated per block.
- Gzip/zstd compression, HTTP/2, and automatic config validation come by
  default — nothing to opt into.
- Caddy's default image entrypoint already runs `caddy run --config
  /etc/caddy/Caddyfile`, so Dockerfiles get slightly shorter too (no `CMD`
  override needed, matching how nginx's image was actually used already).

## Plan

Per repo, replace the runner stage of the Dockerfile and the config file:

- schlussel: `web/nginx.conf` → `web/Caddyfile`, `web/Dockerfile`'s runner
  stage `FROM nginx:alpine` → `FROM caddy:2-alpine`.
- schloss: `nginx.conf` → `Caddyfile`, `Dockerfile`'s runner stage same
  swap.
- kuvert: `web/nginx.conf` → `web/Caddyfile`, `web/Dockerfile`'s runner
  stage same swap.

Caddyfile shape (adjust routes per repo — schlussel/web only needs the
`/auth/callback` + SPA fallback since it *is* the auth UI; schloss and
kuvert/web additionally proxy `/auth/*` to schlussel, kuvert/web also
proxies `/api/*` to kuvert-api):

```
:80 {
	root * /srv
	encode gzip zstd

	@authCallback path /auth/callback*
	handle @authCallback {
		try_files {path} /index.html
		file_server
	}

	handle /auth/* {
		reverse_proxy schlussel:4000
	}

	handle /api/* {
		reverse_proxy kuvert-api:3001
	}

	handle {
		try_files {path} /index.html
		file_server
	}
}
```

Builder stage (node:22-alpine, pnpm build) is untouched — only the runner
stage changes. Static files get copied to `/srv` (Caddy's conventional
root) instead of `/usr/share/nginx/html`.

## Tests

No application test changes — this is infra-only. Verified by actually
building each image and re-running the same real-container smoke tests
already used in Stages 3-5 (register/login through the proxy, SPA fallback
on `/auth/callback`, cross-container reachability) rather than just
reading the Caddyfile.

## Verification

- `docker build` succeeds for all three images.
- Real `docker run` on a shared network reproduces the same
  register/login/callback flow verified in Stages 3-5, now served by
  Caddy instead of nginx.
- `docker compose up` across all three repos still networks correctly.
