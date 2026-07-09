# Schloss Platform — Roadmap

Master index of development stages. Each stage is detailed in `docs/stages/`.
This file is not part of any service's git history — it's a local planning
note shared across sessions (context gets cleared between them; this survives
on disk).

Full background/rationale: see the plan this was generated from, or ask
Claude to re-read `docs/stages/*.md` — each stage file is self-contained.

## GitHub setup (as of 2026-07-09)

- All five repos (`schlussel`, `schloss`, `kuvert`, `tor`, `Hof`) are
  **public** under `zudaR107`.
- `main` is protected via a repository ruleset on the four service repos
  (`schlussel`/`schloss`/`kuvert`/`tor`): PR required (0 approvals needed),
  no force-push, no branch deletion, **no bypass for anyone** including the
  owner. Direct push to `main` is impossible there. `Hof` deliberately has
  **no** ruleset — see its own entry in "Rename, meta-repo, and OSS polish"
  below.
- Labels: `type:feat`, `type:fix`, `type:chore`, `type:test`, `type:docs`
  (same in the four service repos; `Hof` has none, no PR pipeline there).
- Milestones: one per stage/batch, created in each repo it touches — see
  the tables below for issue links, which show the milestone too.
- Project board: [`Schloss Platform`](https://github.com/users/zudaR107/projects/2)
  (user project under zudaR107) — all open issues from the four service
  repos added (`Hof` doesn't use issues).
- CI covers tests, lint, and Docker image publishing to GHCR (`tor` also
  validates its Caddyfile and the full multi-repo `docker compose config`).
  `Hof` has no CI — nothing to test in a docs+submodules repo.
- Git identity for all five repos (local config, not global):
  `zudaR107 <zudin_daniil@mail.ru>`. Commits are GPG-signed
  (`commit.gpgsign=true`, key `42088EAC3C827571`, already registered on the
  GitHub account).

## Status legend
`todo` — not started · `in-progress` — branch/PR exists · `done` — merged

## Stages

| # | Stage | Repo(s) | Issue(s) | Status |
|---|---|---|---|---|
| 0 | [Fix logout navigation](docs/stages/00-fix-logout-navigation.md) | kuvert | [kuvert#2](https://github.com/zudaR107/kuvert/issues/2) | **done** (merged via [kuvert#1](https://github.com/zudaR107/kuvert/pull/1)) |
| 1 | [CI pipelines](docs/stages/01-ci-pipelines.md) | schlussel, schloss, kuvert | [schlussel#1](https://github.com/zudaR107/schlussel/issues/1) · [schloss#1](https://github.com/zudaR107/schloss/issues/1) · [kuvert#3](https://github.com/zudaR107/kuvert/issues/3) | **done** (merged: [schlussel#5](https://github.com/zudaR107/schlussel/pull/5), [schloss#5](https://github.com/zudaR107/schloss/pull/5), [kuvert#9](https://github.com/zudaR107/kuvert/pull/9)) |
| 2 | [Docker-compose networking](docs/stages/02-docker-compose-networking.md) | schlussel, schloss, kuvert | [schlussel#2](https://github.com/zudaR107/schlussel/issues/2) · [schloss#2](https://github.com/zudaR107/schloss/issues/2) · [kuvert#4](https://github.com/zudaR107/kuvert/issues/4) | **done and fully verified** (merged: [schlussel#9](https://github.com/zudaR107/schlussel/pull/9)/[#11](https://github.com/zudaR107/schlussel/pull/11), [schloss#9](https://github.com/zudaR107/schloss/pull/9), [kuvert#13](https://github.com/zudaR107/kuvert/pull/13)/[#15](https://github.com/zudaR107/kuvert/pull/15)) — real `docker compose up` across all three, cross-container JWKS reachability and web-through-nginx confirmed |
| 3 | [Schlussel hosted auth UI](docs/stages/03-schlussel-auth-ui.md) | schlussel | [schlussel#3](https://github.com/zudaR107/schlussel/issues/3) | **done** (merged: [schlussel#18](https://github.com/zudaR107/schlussel/pull/18)) — schlussel is now a pnpm workspace (api at root, new `web/`); both images published and verified live in GHCR |
| 4 | [Kuvert auth migration](docs/stages/04-kuvert-auth-migration.md) | kuvert | [kuvert#5](https://github.com/zudaR107/kuvert/issues/5) | **done** (merged: [kuvert#30](https://github.com/zudaR107/kuvert/pull/30)) — verified with the real 4-container stack; also fixed a JWT_ISSUER default mismatch that broke cross-service auth on real `docker compose up` |
| 5 | [Schloss auth integration](docs/stages/05-schloss-auth-integration.md) | schloss | [schloss#3](https://github.com/zudaR107/schloss/issues/3) | **done** (merged: [schloss#22](https://github.com/zudaR107/schloss/pull/22)) — home page stays public, personalized when logged in; added vitest from scratch; tests written by an independent subagent from spec |
| 6 | [Kuvert UI completion](docs/stages/06-kuvert-ui-completion.md) | kuvert | [kuvert#6](https://github.com/zudaR107/kuvert/issues/6) | **done** (6 PRs: [#33](https://github.com/zudaR107/kuvert/pull/33)-[#38](https://github.com/zudaR107/kuvert/pull/38)) — Modal, Accounts/Debts/Transactions pages, wired-up create buttons, Settings page + new /users/me endpoint |
| 7 | [Budget domain logic](docs/stages/07-budget-domain-logic.md) | kuvert | [kuvert#7](https://github.com/zudaR107/kuvert/issues/7) | **done** (3 PRs: [#39](https://github.com/zudaR107/kuvert/pull/39)-[#41](https://github.com/zudaR107/kuvert/pull/41)) — lazy envelope rollover, recurring goal regeneration, universal CSV import |
| 8 | [Docs and polish](docs/stages/08-docs-and-polish.md) | schlussel, schloss, kuvert | [schlussel#4](https://github.com/zudaR107/schlussel/issues/4) · [schloss#4](https://github.com/zudaR107/schloss/issues/4) · [kuvert#8](https://github.com/zudaR107/kuvert/issues/8) | **done** (merged: [schlussel#26](https://github.com/zudaR107/schlussel/pull/26), [schloss#24](https://github.com/zudaR107/schloss/pull/24), [kuvert#42](https://github.com/zudaR107/kuvert/pull/42)) — README, AGPL-3.0 LICENSE, and CONTRIBUTING.md in all three repos |
| 9 | [Lint in CI](docs/stages/09-lint-in-ci.md) | schlussel, schloss, kuvert | [schlussel#6](https://github.com/zudaR107/schlussel/issues/6) · [schloss#6](https://github.com/zudaR107/schloss/issues/6) · [kuvert#10](https://github.com/zudaR107/kuvert/issues/10) | **done** (merged: [schlussel#12](https://github.com/zudaR107/schlussel/pull/12), [schloss#10](https://github.com/zudaR107/schloss/pull/10), [kuvert#16](https://github.com/zudaR107/kuvert/pull/16)) |
| 10 | [Docker image publishing (GHCR)](docs/stages/10-docker-image-publishing.md) | schlussel, schloss, kuvert | [schlussel#7](https://github.com/zudaR107/schlussel/issues/7) · [schloss#7](https://github.com/zudaR107/schloss/issues/7) · [kuvert#11](https://github.com/zudaR107/kuvert/issues/11) | **done** (merged: [schlussel#17](https://github.com/zudaR107/schlussel/pull/17), [schloss#20](https://github.com/zudaR107/schloss/pull/20), [kuvert#29](https://github.com/zudaR107/kuvert/pull/29)) — all images published and verified live in GHCR |
| 11 | [Dependabot](docs/stages/11-dependabot.md) | schlussel, schloss, kuvert | [schlussel#8](https://github.com/zudaR107/schlussel/issues/8) · [schloss#8](https://github.com/zudaR107/schloss/issues/8) · [kuvert#12](https://github.com/zudaR107/kuvert/issues/12) | **done** (merged: [schlussel#13](https://github.com/zudaR107/schlussel/pull/13), [schloss#11](https://github.com/zudaR107/schloss/pull/11), [kuvert#17](https://github.com/zudaR107/kuvert/pull/17)); vulnerability alerts + auto security fixes also enabled via repo settings (not code) in all three |
| 12 | [nginx to Caddy](docs/stages/12-nginx-to-caddy.md) | schlussel, schloss, kuvert | [schlussel#24](https://github.com/zudaR107/schlussel/issues/24) · [schloss#21](https://github.com/zudaR107/schloss/issues/21) · [kuvert#31](https://github.com/zudaR107/kuvert/issues/31) | **done** (merged: [schlussel#25](https://github.com/zudaR107/schlussel/pull/25), [schloss#23](https://github.com/zudaR107/schloss/pull/23), [kuvert#32](https://github.com/zudaR107/kuvert/pull/32)) — verified with real multi-container runs per repo |

All 13 original stages (0-12) are done. Work continues below without the
"Stage N" numbering/naming convention, per the user's explicit request
after their first hands-on test of the platform (2026-07-09).

## Post-launch fixes and gateway (2026-07-09)

Full context: `/home/zudar/.claude/plans/inherited-exploring-russell.md`.
Four milestones, all closed same day.

| Milestone | Repo(s) | Issue(s) | Status |
|---|---|---|---|
| CHANGELOG | schlussel, schloss, kuvert | [schlussel#27](https://github.com/zudaR107/schlussel/issues/27) · [schloss#25](https://github.com/zudaR107/schloss/issues/25) · [kuvert#43](https://github.com/zudaR107/kuvert/issues/43) | **done** (merged: [schlussel#32](https://github.com/zudaR107/schlussel/pull/32), [schloss#30](https://github.com/zudaR107/schloss/pull/30), [kuvert#46](https://github.com/zudaR107/kuvert/pull/46)) |
| Auth flow hardening | schlussel, schloss | [schlussel#28](https://github.com/zudaR107/schlussel/issues/28) · [schlussel#29](https://github.com/zudaR107/schlussel/issues/29) · [schloss#26](https://github.com/zudaR107/schloss/issues/26) | **done** (merged: [schlussel#33](https://github.com/zudaR107/schlussel/pull/33), [schlussel#35](https://github.com/zudaR107/schlussel/pull/35), [schloss#31](https://github.com/zudaR107/schloss/pull/31)) — login/register unreachable by direct URL, always redirect after auth, password confirm + show/hide toggle, home page now requires auth |
| Single-entry gateway | Tor (new repo), schlussel, schloss, kuvert | [Tor#1](https://github.com/zudaR107/Tor/issues/1) · [schlussel#30](https://github.com/zudaR107/schlussel/issues/30) · [schloss#27](https://github.com/zudaR107/schloss/issues/27) · [kuvert#44](https://github.com/zudaR107/kuvert/issues/44) | **done and verified with a real multi-container run** (Tor bootstrapped directly on `main` - one-time exception, empty repo had no PR base yet; merged: [schlussel#34](https://github.com/zudaR107/schlussel/pull/34), [schloss#32](https://github.com/zudaR107/schloss/pull/32), [kuvert#47](https://github.com/zudaR107/kuvert/pull/47)) — subdomain-routing Caddy gateway (`localhost`/`auth.localhost`/`kuvert.localhost`), one `docker compose up` from `Tor/` starts everything via `include:`, no other container publishes a host port anymore |
| Branding polish | schlussel, schloss, kuvert | [schlussel#31](https://github.com/zudaR107/schlussel/issues/31) (closed, already satisfied) · [schloss#28](https://github.com/zudaR107/schloss/issues/28) · [schloss#29](https://github.com/zudaR107/schloss/issues/29) · [kuvert#45](https://github.com/zudaR107/kuvert/issues/45) | **done** (merged: [schloss#33](https://github.com/zudaR107/schloss/pull/33), [schloss#34](https://github.com/zudaR107/schloss/pull/34), [kuvert#48](https://github.com/zudaR107/kuvert/pull/48)) — distinct Kuvert favicon + tab title fix, Schloss tab title, homepage hero/highlights/GitHub-link polish |

Note: switching from the old standalone per-service `docker compose up` to
running through Tor starts fresh named volumes (`schlussel-data`,
`kuvert-data`) the first time - any account/data created before this batch
landed won't be visible until re-created against the new setup.

## Rename, meta-repo, and OSS polish (2026-07-09)

Full context: `/home/zudar/.claude/plans/inherited-exploring-russell.md`.

- The gateway repo was renamed `Tor` → `tor` (GitHub redirects the old URL;
  local dir and remote updated to match). All references elsewhere fixed
  to the lowercase slug — prose usage of the project name ("Tor" as a
  proper noun) stays capitalized, same convention as Schlüssel/Schloss/
  Kuvert.
- New repo [`Hof`](https://github.com/zudaR107/Hof) — a meta-repo (this
  directory, converted in place) holding only docs (`ROADMAP.md`,
  `docs/stages/`) and git submodules pinning the four service repos.
  Public, **no branch protection**, committed rarely (only to bump a
  submodule pointer at a release) — direct commits, no issue/PR pipeline,
  deliberately different in kind from the four service repos.
- Milestone "Open-source polish" (schlussel, schloss, kuvert, tor): every
  repo got a GitHub description + topics (set directly via `gh repo edit`,
  not a file change) and README polish (license/CI badges, a link to `Hof`
  as the platform overview). Merged:
  [schlussel#38](https://github.com/zudaR107/schlussel/pull/38),
  [schloss#37](https://github.com/zudaR107/schloss/pull/37),
  [kuvert#51](https://github.com/zudaR107/kuvert/pull/51),
  [tor#3](https://github.com/zudaR107/tor/pull/3) (tor's PR also added
  `.env.production.example` for real-domain deployment and a missing
  `.gitignore`).

## Standing workflow (every stage)

- **One issue per stage** (already created, see table below), **one PR per
  issue** — never bundle multiple issues into a single PR.
- One branch per stage, prefixed by type: `fix/...`, `feat/...`, `chore/...`,
  `test/...`.
- Claude creates the branch, implements the feature, then **dispatches a
  fresh subagent with no prior context to write the tests** — briefed with
  a behavioral spec (function signatures, expected behavior) but explicitly
  told not to read the implementation files, so the tests are unbiased and
  can actually catch bugs rather than mirroring whatever the code happens
  to do. Claude runs the resulting tests against the real implementation,
  fixes any real bugs they surface, then commits (GPG-signed, English,
  50/72 format, ASCII-only, no co-author line).
- Claude pushes, opens the PR with **`Closes #N`** in the body (so the
  linked issue auto-closes on merge), labeled, milestoned, assigned to
  `@me`, and **merges it via `gh pr merge` once checks are clean** — no PR
  is left for the user to push/merge manually.
- A stage touching multiple repos produces one branch + one PR (each with
  its own `Closes #N`) per repo, since issues are per-repo.
- Every stage that changes behavior gets tests before being considered done.
- After merge: delete the remote branch, clean up the local branch, update
  this file's status and the stage's own doc.
- Every PR also appends one brief bullet to the repo's own `CHANGELOG.md`
  (under whichever theme section fits, or a new section if none does) —
  not a full commit-by-commit log, just enough to see what shipped.
- New GitHub issue titles do not contain the word "Stage".

## Repo locations

`/home/zudar/Sandbox/Hof/` is itself now the `Hof` meta-repo (git submodules
below, see its own README for details).

- `/home/zudar/Sandbox/Hof/schlussel/` — auth service (Hono API, RS256 JWT via JWKS)
- `/home/zudar/Sandbox/Hof/schloss/` — home page / launcher
- `/home/zudar/Sandbox/Hof/kuvert/` — budget service (api/ + web/, pnpm workspace)
- `/home/zudar/Sandbox/Hof/tor/` — reverse-proxy gateway (Caddyfile + docker-compose only, no app code)

## GitHub account

- Owner: `zudaR107` (git identity: `zudin_daniil@mail.ru`)
- Repos: https://github.com/zudaR107/Hof · https://github.com/zudaR107/schlussel · https://github.com/zudaR107/schloss · https://github.com/zudaR107/kuvert · https://github.com/zudaR107/tor
