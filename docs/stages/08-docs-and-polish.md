# Stage 8 — Documentation and polish

**Repos:** schlussel, schloss, kuvert · **Status:** done

## Problem (confirmed by exploration)

| Repo | README | LICENSE | CONTRIBUTING |
|---|---|---|---|
| schlussel | missing entirely | missing | missing |
| schloss | unedited Vite boilerplate | missing | missing |
| kuvert | only `web/README.md`, also boilerplate; no top-level README | missing | missing |

## Decisions confirmed with the user

- License: **AGPL-3.0**, applied to all three repos (canonical text from
  `gnu.org/licenses/agpl-3.0.txt`, copied verbatim into each `LICENSE`; also
  declared as `"license": "AGPL-3.0-or-later"` in every `package.json`).
- CONTRIBUTING.md: yes, a short one in each repo (getting set up, pre-PR
  checklist, PR conventions, bug/security reporting via GitHub's private
  security-advisory flow rather than a published contact address).
- Explicit instruction: **no git tag or GitHub release** as part of this
  stage — the user wants to manually test the platform first. Deferred
  until they give the go-ahead.

## What was built

- **schlussel**: full README (what it is, platform fit, auth-flow
  explanation of RS256 JWT + JWKS and fragment-based token delivery, local
  dev, env var table, Docker instructions), LICENSE, CONTRIBUTING.md.
  Merged: [schlussel#26](https://github.com/zudaR107/schlussel/pull/26)
  (closes schlussel#4).
- **schloss**: full README (notes it stays fully public as a launcher,
  personalizes when logged in, env var table distinguishing build-time
  `VITE_*` vars from compose build-args), LICENSE, CONTRIBUTING.md.
  Merged: [schloss#24](https://github.com/zudaR107/schloss/pull/24)
  (closes schloss#4).
- **kuvert**: full README (envelope-budgeting feature list, pnpm workspace
  structure, auth-flow note, env var table, Docker instructions), LICENSE,
  CONTRIBUTING.md; deleted the stale `web/README.md` Vite boilerplate.
  Merged: [kuvert#42](https://github.com/zudaR107/kuvert/pull/42)
  (closes kuvert#8).

All three repos' test suites and lint were re-verified green before each
commit; these were docs-only changes with no application-code touched, so
no new tests were needed.
