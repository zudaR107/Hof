# Stage 2 — Docker-compose networking

**Repos:** schlussel, schloss, kuvert (three separate branches/PRs) · **Status:** done, fully verified

Merged: [schlussel#9](https://github.com/zudaR107/schlussel/pull/9) ·
[schloss#9](https://github.com/zudaR107/schloss/pull/9) ·
[kuvert#13](https://github.com/zudaR107/kuvert/pull/13)

**Follow-up bugs found during manual verification** (once
`docker-compose` was installed): neither image actually built or
started — fixed in [schlussel#11](https://github.com/zudaR107/schlussel/pull/11)
and [kuvert#15](https://github.com/zudaR107/kuvert/pull/15). See those
PRs for the four+ distinct bugs (missing `pnpm-workspace.yaml` in build
stages, missing lockfile in runner stage, hardcoded migration paths,
test files breaking the production `tsc` build, a flaky signature-
tampering test, an `erasableSyntaxOnly` violation).

Fully verified end to end: `docker network create schloss-net`, then
schlussel + kuvert-api + kuvert-web brought up via `docker compose up`
in each repo. Confirmed kuvert-api reaches
`http://schlussel:4000/.well-known/jwks.json` from inside its
container, and kuvert-web serves its page through nginx.

## Problem (confirmed by exploration)

- `kuvert/docker-compose.yml` declares both services on `networks: schloss-net:
  external: true` — it assumes a pre-existing external Docker network named
  `schloss-net` that **no compose file actually creates**.
- `schlussel/docker-compose.yml` has no `networks:` block at all — not attached to
  anything.
- `schloss/docker-compose.yml` defines its own **second, separate** schlussel service
  (`image: schlussel:latest`) instead of referencing the real one — also not attached
  to `schloss-net`.
- Net effect: three uncoordinated compose stacks that only nominally agree via
  hardcoded hostnames/env vars (`schlussel`, `SCHLUSSEL_JWKS_URL`, `ALLOWED_ORIGINS`).
  Nothing actually wires them together today.

## Plan

- Decide where `schloss-net` gets created — recommend documenting
  `docker network create schloss-net` as a one-time setup step in each README (simplest,
  no extra top-level compose file to maintain across three repos), but reconsider during
  implementation if it turns out fragile for a self-hoster.
- `schlussel/docker-compose.yml`: add a `networks: schloss-net: external: true` block,
  attach the `schlussel` service to it.
- `schloss/docker-compose.yml`: remove the duplicate inline `schlussel` service
  definition entirely. schloss should just be on `schloss-net` and reach the real
  schlussel (run separately from its own repo) by hostname.
- Confirm hostnames match what kuvert's API already expects
  (`SCHLUSSEL_JWKS_URL=http://schlussel:4000/.well-known/jwks.json`).

## Branch names

- schlussel: `fix/compose-network`
- schloss: `fix/compose-network`
- kuvert: no change expected unless something surfaces during verification

## Tests

No application tests apply — this is infra config.

## Verification (must be manual, not just config review)

- `docker network create schloss-net`
- Bring up schlussel, then kuvert, from their own directories.
- From inside the `kuvert-api` container, confirm it can actually reach
  `http://schlussel:4000/.well-known/jwks.json` (curl from inside the container, or
  check kuvert-api's logs for a successful JWKS fetch on first authenticated request).
