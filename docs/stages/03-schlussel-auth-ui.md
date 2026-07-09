# Stage 3 — Schlussel hosted auth UI

**Repo:** schlussel · new `schlussel/web/` workspace · **Status:** done

Merged: [schlussel#18](https://github.com/zudaR107/schlussel/pull/18)

schlussel is now a pnpm workspace (root = api, `web/` = the new frontend) —
a lighter deviation from literally mirroring kuvert's api+web split, since
moving the already-shipped, well-tested API source into its own
subdirectory would have been a large, risky rewrite for no functional
gain. Adding `web` to `pnpm-workspace.yaml`'s `packages` list gets the same
multi-package capability without touching existing api code, Dockerfile,
or CI history for the API itself. Root's `lint`/`vitest` had to be scoped
explicitly (`oxlint src`, `vitest.config.ts` `exclude: ['web/**']`) since
running from the repo root would otherwise recurse into `web/` with the
wrong plugin config / test environment.

Built as two pages (`LoginPage`, `RegisterPage`) with no router
dependency — path is read directly from `window.location.pathname` in
`main.tsx`. `return_to` validation happens on mount, before any credentials
are entered: an invalid origin renders an `ErrorPage` in place of the form
entirely, rather than only checking after a successful login.

Verified for real, not just via config review: built both Docker images,
ran them on a shared Docker network (using the same container name the
compose file uses, `schlussel`, since nginx's proxy_pass depends on
Docker's DNS resolving that hostname), and curled register + login through
nginx's `/auth/` proxy — got back a real token, a real `Set-Cookie`, and
confirmed both `/login` and `/register` are served (SPA fallback) with
200. Both images published to GHCR and pulled successfully.

## Why

Decision made 2026-07 (during roadmap planning): auth becomes centralized. schlussel
hosts its own Login/Register pages; other services redirect the browser there and get a
token back (OAuth-authorization-code-style flow, adapted for a SPA). This replaces
kuvert's current local `LoginPage.tsx` (removed in Stage 4) and avoids every future
service duplicating its own login form.

## Plan

- New Vite+React frontend at `schlussel/web/`, mirroring kuvert's `api/` + `web/` split
  (schlussel becomes a pnpm workspace the same way kuvert already is).
- Reuse the Schloss design system CSS — copy the same `index.css` pattern already used
  in `kuvert/web/src/index.css` / `schloss/src/index.css` (4 themes, CSS custom
  properties, `.card`/`.btn-primary`/`.input` component classes).
- Pages: `LoginPage.tsx`, `RegisterPage.tsx` — thin clients calling schlussel's own
  existing `/auth/login` and `/auth/register` JSON endpoints. Same-origin, no CORS
  involved (frontend and API now live on the same service).
- Redirect-back flow: both pages accept a `return_to` query param. On success:
  `window.location.href = return_to + '#token=' + accessToken` — a URL fragment, not a
  query string, so the token doesn't leak into server logs or Referer headers on the
  receiving end.

## Security requirement — open redirect guard

The redirect happens entirely client-side; there is no server-side check in the loop.
`return_to` **must** be validated against an allowlist baked into the frontend at build
time: `VITE_ALLOWED_RETURN_ORIGINS` (comma-separated), analogous to schlussel API's
existing `ALLOWED_ORIGINS` env var. If `return_to`'s origin isn't in the allowlist, show
an error page instead of redirecting. Do not skip this — an unchecked client-side
redirect with a token in the fragment is a real open-redirect + token-leak
vulnerability.

## Deployment

- Dockerfile + nginx.conf for `schlussel/web`, mirroring `kuvert/web`'s existing
  pattern (multi-stage build, nginx serves the static build).
- Update `schlussel/docker-compose.yml` to add the new web service and its port.

## Tests

- Component tests for both pages: form rendering, validation errors, successful-login
  redirect behavior, return_to allowlist rejection (error state, no redirect happens).
  Same vitest + testing-library setup already used in `kuvert/web`.

## Verification

- `pnpm test` in `schlussel/web` passes.
- Manual: register a user, confirm redirect back to a whitelisted `return_to` with a
  working token in the fragment; try a non-whitelisted `return_to` and confirm it's
  rejected with an error, not a redirect.
