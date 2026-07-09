# Stage 10 — Docker image publishing to GHCR

**Repos:** schlussel, schloss, kuvert (three separate branches/PRs) · **Status:** done

Merged: [schlussel#17](https://github.com/zudaR107/schlussel/pull/17) ·
[schloss#20](https://github.com/zudaR107/schloss/pull/20) ·
[kuvert#29](https://github.com/zudaR107/kuvert/pull/29)

Images: `ghcr.io/zudar107/schlussel`, `ghcr.io/zudar107/schloss`,
`ghcr.io/zudar107/kuvert-api`, `ghcr.io/zudar107/kuvert-web` - all
tagged `:latest` and `:<sha>`. Owner hardcoded lowercase in tags
(`zudar107`) since ghcr.io requires lowercase and
`github.repository_owner` preserves the account's display case
(`zudaR107`).

**Bug found and fixed separately** ([schloss#19](https://github.com/zudaR107/schloss/pull/19)):
schloss's Dockerfile never declared `ARG VITE_KUVERT_URL`, so the value
docker-compose passed as a build arg was silently discarded and the
built image always linked to the hardcoded `localhost:5174` default.

All four images confirmed live and pullable from GHCR after each
publish workflow ran on `main`.

A Dependabot security alert (moderate, `esbuild@0.18.20` via
drizzle-kit, dev-only tooling) surfaced in kuvert as a side effect of
Stage 11's alerts being enabled - left for Dependabot to resolve
automatically rather than adding a manual override, given the low real
risk (drizzle-kit doesn't expose a network dev server in normal use).

## Why

This is self-hosted software, not a SaaS with a known deploy target - there
is no server to SSH into and restart. The practical equivalent of "deploy
automation" here is: build and publish Docker images to GitHub Container
Registry (GHCR) on every merge to `main`, so a self-hoster's update process
is just `docker compose pull && docker compose up -d`. Actual deployment to
any specific host is out of scope until such a host/access exists.

## Plan

- New `.github/workflows/publish.yml` per repo, triggered on push to `main`
  (after tests pass - `needs: test` or a separate workflow gated on the
  test workflow's success).
- Build via the existing Dockerfiles already in each repo:
  - schlussel: single Dockerfile (API only) - and, once Stage 3 lands, also
    `schlussel/web/Dockerfile`.
  - schloss: single Dockerfile.
  - kuvert: `api/Dockerfile` and `web/Dockerfile` - two images.
- Push to `ghcr.io/zudar107/<service>[-<component>]:latest` and
  `:<git-sha>` tags. Use `docker/build-push-action` with
  `permissions: packages: write` - GHCR authenticates via the workflow's
  own `GITHUB_TOKEN`, no extra secrets needed for a repo the same user
  owns.
- Decide image visibility (public vs private) - likely public, matching
  the repos themselves, but confirm before first publish since GHCR
  visibility is a separate setting from repo visibility.

## Tests

No new application tests - this is a build/publish pipeline. Verify by
actually pulling the published image and confirming it starts correctly
(`docker run` smoke test, or reuse the existing docker-compose setup from
Stage 2 pointed at the published tag instead of a local build).

## Verification

After the workflow runs once, `docker pull ghcr.io/zudaR107/<image>:latest`
succeeds and the container starts and responds on its expected port.
