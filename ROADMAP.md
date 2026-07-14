# Schloss Platform — Roadmap

Master index of development stages. This file is not part of any service's
git history — it's a local planning note shared across sessions (context
gets cleared between them; this survives on disk).

Per-stage detail files (`docs/stages/*.md`) that existed for stages 0-12
have been removed now that all of them are done — the table below plus
each linked issue/PR already carries what's worth keeping. Full
background/rationale for later batches: see the plan files referenced under
each section below.

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
| 0 | Fix logout navigation | kuvert | [kuvert#2](https://github.com/zudaR107/kuvert/issues/2) | **done** (merged via [kuvert#1](https://github.com/zudaR107/kuvert/pull/1)) |
| 1 | CI pipelines | schlussel, schloss, kuvert | [schlussel#1](https://github.com/zudaR107/schlussel/issues/1) · [schloss#1](https://github.com/zudaR107/schloss/issues/1) · [kuvert#3](https://github.com/zudaR107/kuvert/issues/3) | **done** (merged: [schlussel#5](https://github.com/zudaR107/schlussel/pull/5), [schloss#5](https://github.com/zudaR107/schloss/pull/5), [kuvert#9](https://github.com/zudaR107/kuvert/pull/9)) |
| 2 | Docker-compose networking | schlussel, schloss, kuvert | [schlussel#2](https://github.com/zudaR107/schlussel/issues/2) · [schloss#2](https://github.com/zudaR107/schloss/issues/2) · [kuvert#4](https://github.com/zudaR107/kuvert/issues/4) | **done and fully verified** (merged: [schlussel#9](https://github.com/zudaR107/schlussel/pull/9)/[#11](https://github.com/zudaR107/schlussel/pull/11), [schloss#9](https://github.com/zudaR107/schloss/pull/9), [kuvert#13](https://github.com/zudaR107/kuvert/pull/13)/[#15](https://github.com/zudaR107/kuvert/pull/15)) — real `docker compose up` across all three, cross-container JWKS reachability and web-through-nginx confirmed |
| 3 | Schlussel hosted auth UI | schlussel | [schlussel#3](https://github.com/zudaR107/schlussel/issues/3) | **done** (merged: [schlussel#18](https://github.com/zudaR107/schlussel/pull/18)) — schlussel is now a pnpm workspace (api at root, new `web/`); both images published and verified live in GHCR |
| 4 | Kuvert auth migration | kuvert | [kuvert#5](https://github.com/zudaR107/kuvert/issues/5) | **done** (merged: [kuvert#30](https://github.com/zudaR107/kuvert/pull/30)) — verified with the real 4-container stack; also fixed a JWT_ISSUER default mismatch that broke cross-service auth on real `docker compose up` |
| 5 | Schloss auth integration | schloss | [schloss#3](https://github.com/zudaR107/schloss/issues/3) | **done** (merged: [schloss#22](https://github.com/zudaR107/schloss/pull/22)) — home page stays public, personalized when logged in; added vitest from scratch; tests written by an independent subagent from spec |
| 6 | Kuvert UI completion | kuvert | [kuvert#6](https://github.com/zudaR107/kuvert/issues/6) | **done** (6 PRs: [#33](https://github.com/zudaR107/kuvert/pull/33)-[#38](https://github.com/zudaR107/kuvert/pull/38)) — Modal, Accounts/Debts/Transactions pages, wired-up create buttons, Settings page + new /users/me endpoint |
| 7 | Budget domain logic | kuvert | [kuvert#7](https://github.com/zudaR107/kuvert/issues/7) | **done** (3 PRs: [#39](https://github.com/zudaR107/kuvert/pull/39)-[#41](https://github.com/zudaR107/kuvert/pull/41)) — lazy envelope rollover, recurring goal regeneration, universal CSV import |
| 8 | Docs and polish | schlussel, schloss, kuvert | [schlussel#4](https://github.com/zudaR107/schlussel/issues/4) · [schloss#4](https://github.com/zudaR107/schloss/issues/4) · [kuvert#8](https://github.com/zudaR107/kuvert/issues/8) | **done** (merged: [schlussel#26](https://github.com/zudaR107/schlussel/pull/26), [schloss#24](https://github.com/zudaR107/schloss/pull/24), [kuvert#42](https://github.com/zudaR107/kuvert/pull/42)) — README, AGPL-3.0 LICENSE, and CONTRIBUTING.md in all three repos |
| 9 | Lint in CI | schlussel, schloss, kuvert | [schlussel#6](https://github.com/zudaR107/schlussel/issues/6) · [schloss#6](https://github.com/zudaR107/schloss/issues/6) · [kuvert#10](https://github.com/zudaR107/kuvert/issues/10) | **done** (merged: [schlussel#12](https://github.com/zudaR107/schlussel/pull/12), [schloss#10](https://github.com/zudaR107/schloss/pull/10), [kuvert#16](https://github.com/zudaR107/kuvert/pull/16)) |
| 10 | Docker image publishing (GHCR) | schlussel, schloss, kuvert | [schlussel#7](https://github.com/zudaR107/schlussel/issues/7) · [schloss#7](https://github.com/zudaR107/schloss/issues/7) · [kuvert#11](https://github.com/zudaR107/kuvert/issues/11) | **done** (merged: [schlussel#17](https://github.com/zudaR107/schlussel/pull/17), [schloss#20](https://github.com/zudaR107/schloss/pull/20), [kuvert#29](https://github.com/zudaR107/kuvert/pull/29)) — all images published and verified live in GHCR |
| 11 | Dependabot | schlussel, schloss, kuvert | [schlussel#8](https://github.com/zudaR107/schlussel/issues/8) · [schloss#8](https://github.com/zudaR107/schloss/issues/8) · [kuvert#12](https://github.com/zudaR107/kuvert/issues/12) | **done** (merged: [schlussel#13](https://github.com/zudaR107/schlussel/pull/13), [schloss#11](https://github.com/zudaR107/schloss/pull/11), [kuvert#17](https://github.com/zudaR107/kuvert/pull/17)); vulnerability alerts + auto security fixes also enabled via repo settings (not code) in all three |
| 12 | nginx to Caddy | schlussel, schloss, kuvert | [schlussel#24](https://github.com/zudaR107/schlussel/issues/24) · [schloss#21](https://github.com/zudaR107/schloss/issues/21) · [kuvert#31](https://github.com/zudaR107/kuvert/issues/31) | **done** (merged: [schlussel#25](https://github.com/zudaR107/schlussel/pull/25), [schloss#23](https://github.com/zudaR107/schloss/pull/23), [kuvert#32](https://github.com/zudaR107/kuvert/pull/32)) — verified with real multi-container runs per repo |

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
| Single-entry gateway | tor (new repo, originally created as `Tor` - see the rename entry below), schlussel, schloss, kuvert | [tor#1](https://github.com/zudaR107/tor/issues/1) · [schlussel#30](https://github.com/zudaR107/schlussel/issues/30) · [schloss#27](https://github.com/zudaR107/schloss/issues/27) · [kuvert#44](https://github.com/zudaR107/kuvert/issues/44) | **done and verified with a real multi-container run** (tor bootstrapped directly on `main` - one-time exception, empty repo had no PR base yet; merged: [schlussel#34](https://github.com/zudaR107/schlussel/pull/34), [schloss#32](https://github.com/zudaR107/schloss/pull/32), [kuvert#47](https://github.com/zudaR107/kuvert/pull/47)) — subdomain-routing Caddy gateway (`localhost`/`auth.localhost`/`kuvert.localhost`), one `docker compose up` from `tor/` starts everything via `include:`, no other container publishes a host port anymore |
| Branding polish | schlussel, schloss, kuvert | [schlussel#31](https://github.com/zudaR107/schlussel/issues/31) (closed, already satisfied) · [schloss#28](https://github.com/zudaR107/schloss/issues/28) · [schloss#29](https://github.com/zudaR107/schloss/issues/29) · [kuvert#45](https://github.com/zudaR107/kuvert/issues/45) | **done** (merged: [schloss#33](https://github.com/zudaR107/schloss/pull/33), [schloss#34](https://github.com/zudaR107/schloss/pull/34), [kuvert#48](https://github.com/zudaR107/kuvert/pull/48)) — distinct Kuvert favicon + tab title fix, Schloss tab title, homepage hero/highlights/GitHub-link polish |

Note: switching from the old standalone per-service `docker compose up` to
running through tor starts fresh named volumes (`schlussel-data`,
`kuvert-data`) the first time - any account/data created before this batch
landed won't be visible until re-created against the new setup.

## Rename, meta-repo, and OSS polish (2026-07-09)

Full context: `/home/zudar/.claude/plans/inherited-exploring-russell.md`.

- The gateway repo's slug was renamed to lowercase `tor` (GitHub redirects
  the old URL; local dir and remote updated to match). Unlike
  Schlüssel/Schloss/Kuvert, the user wants this project name written
  lowercase everywhere, including prose and sentence starts, not just the
  URL/slug — fixed across all four service repos and this one.
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

## Authorization Code + PKCE (2026-07-09)

Full design: `/home/zudar/.claude/plans/inherited-exploring-russell.md`.
Replaces the old "token in URL fragment" login handoff with OAuth2
Authorization Code + PKCE, so the access token no longer travels through
any URL at all. Milestone "Authorization Code flow (PKCE)"
(schlussel, schloss, kuvert), all closed same day:

- **schlussel** (API + web, one PR — tightly coupled): new `auth_codes`
  table; `POST /auth/login` optionally accepts a PKCE `codeChallenge` and,
  when given, issues a 60s single-use code instead of a token; new
  `POST /auth/token` exchanges the code + verifier for the real
  `{accessToken, user}`. LoginPage/RegisterPage now require a
  `code_challenge` alongside `return_to` to render, and redirect with
  `?code=` instead of `#token=`. Merged:
  [schlussel#41](https://github.com/zudaR107/schlussel/pull/41) — this PR
  also fixed a real bug the independent test-writing agent found: the
  cross-navigation links between the login/register pages only forwarded
  `return_to`, silently dropping `code_challenge` and breaking the other
  page's guard mid-flow.
- **schloss** and **kuvert**: `buildSchluesselLoginUrl` is now `async` —
  generates a PKCE verifier, stores it in `sessionStorage`, sends its
  challenge alongside `return_to`. `AuthCallbackPage` exchanges the
  returned code (from the query string) for the token via
  `POST /auth/token`, which now returns `{accessToken, user}` in one
  response — no more separate `/auth/me` call needed. Merged:
  [schloss#40](https://github.com/zudaR107/schloss/pull/40),
  [kuvert#54](https://github.com/zudaR107/kuvert/pull/54).
- **Verified live**, end-to-end, against the real running Docker stack
  (not just unit tests) by a dedicated verification agent: full round
  trip, the resulting token accepted by kuvert-api via independent JWKS
  verification, single-use enforcement, verifier-binding (wrong verifier
  rejected), code expiry (60s) enforced, and malformed/half-supplied PKCE
  params hard-rejected rather than silently downgraded to a plain login.
  No gaps found.

## Community health files (2026-07-10)

`CODE_OF_CONDUCT.md`, `SECURITY.md` (GitHub private-reporting flow,
scope tailored per repo), issue templates (bug report, feature request),
and a pull request template added to all five repos. Also removed
`docs/stages/*.md` from Hof now that all 13 original stages are done.
Merged: [schlussel#43](https://github.com/zudaR107/schlussel/pull/43),
[schloss#42](https://github.com/zudaR107/schloss/pull/42),
[kuvert#56](https://github.com/zudaR107/kuvert/pull/56),
[tor#6](https://github.com/zudaR107/tor/pull/6).

## Gateway HTTPS origin defaults (2026-07-10)

Found by the user doing real hands-on testing through the tor gateway
after trusting its local CA in Firefox: `docker-compose.yml`'s default
origins (`ALLOWED_ORIGINS`, `VITE_ALLOWED_RETURN_ORIGINS`,
`VITE_SCHLUSSEL_URL`, `VITE_KUVERT_URL`, `VITE_DEFAULT_APP_URL`) still
assumed `http://`, but tor's Caddy auto-upgrades everything to HTTPS —
broke the return_to allowlist ("Небезопасный адрес возврата") and CORS
for anyone actually running the real stack rather than curling internal
ports directly. Fixed all `http://` defaults to `https://` for the
gateway-routed hostnames (`localhost`, `auth.localhost`,
`kuvert.localhost`); left internal Docker-network URLs (e.g.
`http://schlussel:4000` for kuvert-api's JWKS fetch) and container-local
healthchecks alone, since those never go through the gateway. Merged:
[schlussel#45](https://github.com/zudaR107/schlussel/pull/45),
[kuvert#58](https://github.com/zudaR107/kuvert/pull/58),
[schloss#44](https://github.com/zudaR107/schloss/pull/44).

## ALLOWED_ORIGINS variable collision (2026-07-10)

Follow-up to the batch above: tor's docker-compose.yml `include:`s
schlussel and kuvert under one shared `.env`. Both services' own
docker-compose.yml used the exact same outer substitution variable name
`ALLOWED_ORIGINS` for two different intended CORS allowlists, so setting
it in `tor/.env` silently fed the same value into both and clobbered
kuvert-api's own default (`https://localhost,https://kuvert.localhost`)
with schlussel's. Not currently visible as a symptom (kuvert-web proxies
`/api/*` same-origin through its own Caddy, so no browser CORS preflight
ever hits kuvert-api directly), but a real latent misconfiguration.
Renamed the outer variable per-service - `SCHLUSSEL_ALLOWED_ORIGINS` and
`KUVERT_ALLOWED_ORIGINS` - matching the naming convention already used
for `KUVERT_URL`/`SCHLUSSEL_WEB_URL`; the container-internal env var name
each app reads is unchanged (`ALLOWED_ORIGINS`). Merged:
[schlussel#47](https://github.com/zudaR107/schlussel/pull/47),
[kuvert#60](https://github.com/zudaR107/kuvert/pull/60),
[tor#10](https://github.com/zudaR107/tor/pull/10).

## User feedback batch: SSO, UI consistency, kuvert UX (2026-07-10)

Seven issues from the user's first real hands-on walkthrough after the
gateway HTTPS fixes, across two milestones plus two standalone kuvert
issues. All closed same day.

- **Single sign-on across services** (milestone, schlussel only):
  [schlussel#48](https://github.com/zudaR107/schlussel/pull/50) -
  root cause: the refresh-token cookie had no `Domain` attribute, so it
  was host-only, scoped to whichever app's own Caddy happened to proxy
  the `/auth/token`/`/auth/refresh` call. A session started on schloss
  never carried over to kuvert. Added optional `COOKIE_DOMAIN`, wired to
  reuse tor's existing `DOMAIN` var - no client-side changes needed
  anywhere, since each app's silent-refresh-on-mount logic already
  existed and just needed the cookie to actually be visible.
- **Unified UI shell across services** (milestone, schloss + schlussel +
  kuvert): schloss's Header/Footer extracted into reusable components
  ([schloss#47](https://github.com/zudaR107/schloss/pull/49), also fixed
  `/favicon.svg` being cached `immutable` for a year); the hero
  illustration (an 8-bit indexed PNG, visibly washed out) and the
  favicon (an abstract gradient-blob shape that read as a generic
  lightning bolt) both replaced with hand-authored on-brand SVGs
  ([schloss#48](https://github.com/zudaR107/schloss/pull/50)); the
  previously bare login/register pages got a matching header/footer
  ([schlussel#49](https://github.com/zudaR107/schlussel/pull/51)); the
  kuvert sidebar now shows the signed-in user's name/email and gained a
  footer ([kuvert#61](https://github.com/zudaR107/kuvert/pull/65)).
- **kuvert, standalone (no milestone)**: the sidebar's tiny 24x24 toggle
  button replaced with a full-edge drag-to-resize handle, snapping shut
  below a threshold, remembering the last width in localStorage
  ([kuvert#62](https://github.com/zudaR107/kuvert/pull/64)); Budget and
  Accounts empty-state copy expanded to explain the envelope-budgeting
  distinction between the two, each page cross-referencing the other
  ([kuvert#63](https://github.com/zudaR107/kuvert/pull/66)).

Every PR in this batch had tests written by a fresh independent
subagent from a behavioral spec (no implementation access) - no real
bugs found, only expected staleness in one pre-existing schloss test
(the old `<img>`-based hero assertion), fixed by a separate small agent
dispatch. Two earlier batches (community health files, gateway HTTPS
fixes) were retroactively corrected to match this repo's actual label
convention (`type:*`, not GitHub's generic `bug`/`documentation`) and
backfilled into the "Schloss Platform" project board and proper
milestones - they had drifted from the established convention.

## SSO correction: silent re-auth, not a shared cookie (2026-07-10)

The `COOKIE_DOMAIN` fix from the batch above turned out not to work:
confirmed live that `localhost` has no embedded dot, so both curl and
real browsers treat a dotless `Domain` value as a suffix boundary
rather than a shareable registrable domain (the same defensive rule
that stops a site setting a cookie for `.com`) - the cookie never
actually got attached to a request against a different `*.localhost`
subdomain. Reopened milestone "Single sign-on across services",
reverted `COOKIE_DOMAIN` entirely (schlussel#52,
[PR#53](https://github.com/zudaR107/schlussel/pull/53)) rather than
leave dead code around, and replaced it with the standard
centralized-IdP silent-reauth pattern: `/auth/login`'s PKCE branch now
also establishes a same-origin session (this endpoint is only ever
called from schlussel's own hosted login page), and `/auth/refresh`
can issue a PKCE code from that session instead of an access token.
The login page tries this first, silently, before ever showing the
credentials form. **Verified live** against the real running stack:
full protocol trace (register, login on `auth.localhost`, silent
`/auth/refresh` with no password, code redemption on
`kuvert.localhost`) confirmed working end to end. Tests: 142/142 API,
71/71 web, no real bugs found across two independent dispatches.

## Reduce SSO redirect flicker (2026-07-10)

Follow-up to the fix above: the first-time cross-service redirect
chain (app -> `auth.localhost` -> app) is 2-3 full page loads, each
flashing the browser's default (unthemed, usually white) background
before CSS/JS finishes loading - reported by the user as "the screen
blinks," confirmed to only happen on the *first* navigation per
session (subsequent navigations reuse the in-memory access token, no
redirect chain). A true zero-flicker fix would need a hidden-iframe
SSO pattern, deliberately avoided: modern browsers (Safari ITP,
Firefox ETP) increasingly block third-party cookie access inside
iframes for exactly this scenario, which would make it flaky across
browsers. Instead, applied the standard no-FOUC-dark-mode technique to
all three frontends - a synchronous inline script in each
`index.html`'s `<head>` restoring the stored theme before first paint,
plus themed blank divs (instead of `return null`) during every
transient loading state in the chain (schlussel's login page while
checking for a session, schloss/kuvert's auth callback while
exchanging the code). schloss also had an unrelated pre-existing gap
here - it only applied the stored theme inside HomePage's `useEffect`
(after first paint) instead of eagerly in `main.tsx` like the other
two - fixed to match. Merged:
[schlussel#55](https://github.com/zudaR107/schlussel/pull/55),
[schloss#52](https://github.com/zudaR107/schloss/pull/52),
[kuvert#68](https://github.com/zudaR107/kuvert/pull/68).

## kuvert: remove tab-switch flash via route loaders (2026-07-10)

Separate report from the same user testing session: switching between
kuvert's own tabs (Budget/Accounts/Goals/...) also flickered, but
client-side routing was already correct (Layout/sidebar don't
remount) - the cause was each page fetching its own data only after
mounting, showing "empty/skeleton, then content pops in" on every
first visit to a tab per session. Fixed with TanStack Router loaders
prefetching each route's data via `queryClient.ensureQueryData`
before the transition completes (the officially recommended
integration pattern for these two libraries together, not a
workaround) - required extracting the `QueryClient` into its own
module (`src/lib/queryClient.ts`) so the router's loaders and the
app's `QueryClientProvider` share the exact same instance. Loader
failures are swallowed rather than surfacing an error boundary.
Tests: 237/237, 12 new, no mismatches. Merged:
[kuvert#70](https://github.com/zudaR107/kuvert/pull/70).

## kuvert: sidebar click-to-toggle, clipped Footer (2026-07-10)

Two follow-ups from the same testing round. The sidebar's small round
chevron toggle button was removed - clicking anywhere on the sidebar
that isn't a nav link, the theme button, the logout button, or the
user identity block now toggles collapsed/expanded, with each
interactive child stopping the click from bubbling up so its own
action never also toggles the sidebar.

Also found and fixed a real bug via user testing in a fresh private
Firefox window (ruling out caching as the earlier hypothesis): the
Footer added in the "Unified UI shell" batch was never actually
visible. `<main>` had `flex: 1; overflowY: auto` but no
`min-height: 0` - a flex item defaults to `min-height: auto`, so on
any page with enough content it grows past the viewport instead of
scrolling within its allotted space, and the parent's
`overflow: hidden` clips the Footer that comes after it in the flex
column - not a "needs scrolling" issue, genuinely unreachable. Same
fix applied to `<nav>` defensively. Tests: 239/239, 6 rewritten to
match the button's removal, no mismatches. Merged:
[kuvert#73](https://github.com/zudaR107/kuvert/pull/73).

## kuvert: drag-revert bug, header with home/settings link (2026-07-10)

Four more items from the same testing round, all in kuvert (plus one
small cross-repo wiring PR in tor):

- **Drag-resize reverting** (kuvert#74,
  [PR#75](https://github.com/zudaR107/kuvert/pull/75)): browsers fire a
  synthetic click on mouseup right after a drag if the pointer ends up
  back over an element inside the sidebar (common for a small/moderate
  drag) - that phantom click was bubbling to the click-to-toggle handler
  from the batch above and immediately collapsing the sidebar, silently
  discarding the width just dragged to. A large drag doesn't trigger it
  because the pointer ends up over the main content area, outside the
  sidebar - exactly matching the reported "only happens if you don't
  drag it all the way." Fixed by tracking whether a real drag happened
  and suppressing the next click if so, cleared via `setTimeout(fn, 0)`
  so only the same-tick synthetic click gets suppressed.
- **tor: `SCHLOSS_URL`** (tor#11,
  [PR#12](https://github.com/zudaR107/tor/pull/12)): added to
  `.env.example`/`.env.production.example` - no existing var pointed
  back toward schloss's home, needed for the header below.
- **kuvert: header with home/settings link** (kuvert#76,
  [PR#77](https://github.com/zudaR107/kuvert/pull/77)): no way to see a
  persistent header, get back to schloss's home page, or discover
  settings - the sidebar carries identity/logout/a Settings nav link,
  but isn't a "header" the way schloss has one, offers no way back home,
  and is hidden entirely on narrow/mobile viewports. Added a Header,
  always visible (desktop and mobile), with a link back to schloss
  (`VITE_SCHLOSS_URL`, reading tor's new var), the user's name, and
  Settings/logout links - alongside, not replacing, the sidebar's own
  controls. Note: kuvert's existing Settings page only covers currency -
  there's no platform-wide "account settings" (name/email/password)
  anywhere yet; this links to the existing page as the best available
  fix for now.
- **Build-breaking unused import** (kuvert
  [PR#78](https://github.com/zudaR107/kuvert/pull/78)): the independent
  test agent's new `Header.test.tsx` had an unused `screen` import -
  `pnpm test` (vitest) passed fine, but `pnpm run build`
  (`tsc -b && vite build`, what the Docker image actually runs) failed
  on it under this project's strict tsconfig. Caught during the next
  `docker compose build`, before it reached the live stack - a reminder
  to verify the actual build command after a test-writing agent's
  changes land, not just `pnpm test`.

Every behavior-changing PR in this batch had tests written by a fresh
independent subagent from a behavioral spec (no implementation access);
no real bugs found beyond the two above (both found through the
project's own live-stack/build verification, not the agents). Tests
after this batch: kuvert 247/247.

## kuvert: shorten empty-state copy; CI-wide pnpm pin (2026-07-13)

The Budget/Accounts empty-state copy added earlier was noticeably
longer than every other tab's one-sentence hint (Goals, Debts,
Transactions) - shortened both back down to one sentence, keeping the
Budget<->Accounts cross-reference
([kuvert#79](https://github.com/zudaR107/kuvert/issues/79), merged as
part of [kuvert#80](https://github.com/zudaR107/kuvert/pull/80)).

That PR's CI failed on an unrelated infrastructure issue: `pnpm`
11.12.0 shipped with a bug in its own self-installer, and
`pnpm/action-setup`'s unpinned `version: 11` self-updates to whatever
the latest 11.x is at run time - broke every workflow run in every JS
repo, regardless of what changed. Pinned to `11.7.0` (already cached,
working) in kuvert as part of the same PR to unblock it, then
proactively applied the identical fix to schlussel
([PR#57](https://github.com/zudaR107/schlussel/pull/57)) and schloss
([PR#54](https://github.com/zudaR107/schloss/pull/54)) before it could
block anything there too.

## schloss-ui: shared design system, planning (2026-07-13)

The user's explicit ask from earlier feedback ("unify the interface, but
without making the services identical copies") comes back around: not a
quick fix this time, but a real shared package. Design decided before
any code:

- **What's already shared**: confirmed the core tokens (`--radius-*`,
  `--shadow-*`, `--font-sans`, neutrals, `--success/warning/danger/info`)
  are already byte-identical across schloss/schlussel/kuvert - drifted
  apart only by hand-copying, not a real design gap.
- **What actually makes the services feel like copies**: `--accent` is
  the same blue (`#3b82f6`) everywhere, including in schloss's own UI
  despite its logo already being purple (`#863bff`) - an internal
  inconsistency, not just a cross-service one.
- **The fix**: keep the shared "core" tokens as the single source of
  truth in a new package; keep the *token names* consumers already use
  unchanged (`--accent` etc.) but let each service set its own accent
  value on top - schloss `#863bff` (now matches its own logo), schlussel
  `#3b82f6` (keeps the platform's original blue as its own signature),
  kuvert `#0d9488` teal (a finance association, deliberately distinct
  from the shared `--success` green so the two don't get confused).
- **Icons/logos**: the badge pattern (colored rounded-square + white
  line-icon) is already informally consistent - formalized as a
  documented contract (`strokeWidth: 2`, matching lucide-react's default
  already used for every other icon) rather than centralizing the actual
  brand mark SVGs, which stay one per service on purpose - that's the
  "family, not copies" part.
- **Packaging**: new repo [`schloss-ui`](https://github.com/zudaR107/schloss-ui)
  (public, AGPL-3.0, same branch-protection ruleset as the other service
  repos, bootstrapped directly on `main` - one-time exception, empty repo
  had no PR base yet, same precedent as tor's original bootstrap),
  published as `@zudar107/schloss-ui` to GitHub Packages on tagged
  release (not on every merge like the platform's Docker images - a
  package needs a fixed, reproducible version to pin against), installed
  as a normal dependency so Dependabot can track its version like any
  other package.

First pass at milestone "Shared UI system" filed seven issues (design
tokens, publish pipeline, Header/Footer, one adoption issue per
consumer) - design and issue creation only, no implementation yet.

## schloss-ui: deep visual draft, plan rewritten (2026-07-13)

The user pushed back on the first pass: they didn't want the *current*
UI packaged as-is - they wanted to actually improve it first, together,
before locking anything into a shared package. Two concrete complaints
kicked this off: the Header is too wordy (a text label next to every
icon), and pages feel empty in a way plain minimalism doesn't excuse.

Worked this as an actual design review rather than more back-and-forth
in text - built an HTML artifact (before/after mockups, using the
platform's real token values so they're accurate previews, not
abstractions) and iterated on it live across three rounds: Header/empty
states/buttons, then forms/modals/badges/segmented filters, then
icon-size-and-color rules/sign-colored numbers/a toast pattern (the one
genuinely new addition, not a fix to something existing). Landed on:

- **Header**: the logo *is* the home link (no separate "На главную"
  text); an avatar circle replaces the visible user name; settings/logout
  become icon-only with a `title` tooltip instead of icon+text.
- **Cards/empty states**: tinted icon badges instead of flat squares or
  raw emoji; summary-strip stat tiles above tables instead of jumping
  straight from header to empty space (kuvert's Budget page already has
  one - "Осталось распределить" - the pattern generalizes).
- **Buttons/badges/filters**: one Button (primary/secondary/ghost/danger),
  one Badge shape replacing three ad hoc styles, a SegmentedControl
  replacing the Debts page's two-separate-buttons filter.
- **Forms/modals**: a visible focus ring, a currency prefix slot, actual
  error copy; modal titles get a context icon, footers get a clear
  primary action instead of two equal-weight buttons.
- **Icons**: `strokeWidth: 2` always, a 4-step size scale (14/16/20/28px)
  tied to specific contexts, a 4-state color rule instead of eyeballing.
- **Numbers**: found a *real* inconsistency, not an invented one - kuvert's
  Budget "Available" column already colors by sign, but account balances
  and transaction amounts elsewhere don't. One `Amount` rule fixes both.
- **Toast**: no existing pattern to fix - proposed as a new addition,
  first real usage wired into kuvert's save flows once Modal adoption
  lands there.

**Old plan erased, not just closed** - all seven issues from the first
pass deleted outright (GraphQL `deleteIssue`, not closed-with-a-comment)
since they no longer describe real work; superseded by 13 new, far more
concrete issues covering the full draft:

- schloss-ui: [#5](https://github.com/zudaR107/schloss-ui/issues/5) tokens
  + accent contract, [#6](https://github.com/zudaR107/schloss-ui/issues/6)
  publish pipeline, [#7](https://github.com/zudaR107/schloss-ui/issues/7)
  Header/Footer/EmptyState, [#8](https://github.com/zudaR107/schloss-ui/issues/8)
  Button/Badge/SegmentedControl, [#9](https://github.com/zudaR107/schloss-ui/issues/9)
  Field/Modal, [#10](https://github.com/zudaR107/schloss-ui/issues/10)
  StatTile/Amount/Sparkline, [#11](https://github.com/zudaR107/schloss-ui/issues/11)
  Toast, [#12](https://github.com/zudaR107/schloss-ui/issues/12) icon docs.
- schloss: [#56](https://github.com/zudaR107/schloss/issues/56).
- schlussel: [#59](https://github.com/zudaR107/schlussel/issues/59).
- kuvert (split into three - largest surface of the three consumers):
  [#82](https://github.com/zudaR107/kuvert/issues/82) Header/Footer/EmptyState/accent,
  [#83](https://github.com/zudaR107/kuvert/issues/83) buttons/badges/filters/cards/numbers,
  [#84](https://github.com/zudaR107/kuvert/issues/84) forms/modals/toasts.

Design and issue creation only in this batch too - implementation is
next. The review artifact itself will be deleted once the real
components exist and match it (no tool to do that automatically - a
manual follow-up once implementation lands).

## schloss-ui: v0.1.0 - tokens and all 12 components shipped (2026-07-13)

Implemented all 8 schloss-ui-side issues from the plan above, each its
own PR through the standing pipeline, plus one fix found along the way:

- [#6](https://github.com/zudaR107/schloss-ui/pull/13) package skeleton
  (tsup, vitest, oxlint) and a tag-triggered GitHub Packages publish
  workflow.
- [#5](https://github.com/zudaR107/schloss-ui/pull/14) shared tokens
  (`tokens.css`) - radius/shadow/font/neutral/semantic colors for
  light/dark/oled/sepia, with `--accent`/`--accent-hover`/
  `--accent-muted`/`--accent-text`/`--sidebar-accent` deliberately left
  out and documented as a per-service contract instead.
- [#7](https://github.com/zudaR107/schloss-ui/pull/15) `Header`,
  `Footer`, `EmptyState`.
- [#8](https://github.com/zudaR107/schloss-ui/pull/16) `Button`,
  `Badge` (added `--badge-*` tokens - text shades intentionally differ
  from `--success`/`--danger` etc for contrast against a muted
  background), `SegmentedControl`.
- [#17](https://github.com/zudaR107/schloss-ui/pull/18) fix, found right
  after #8: `Button`, `Header`'s icon buttons, and `SegmentedControl`'s
  inactive segments render from inline `style` objects, which can't
  express `:hover` - all three shipped with zero hover feedback versus
  the CSS-class-based buttons they replace. Fixed with an internal
  `useHover` hook.
- [#9](https://github.com/zudaR107/schloss-ui/pull/19) `Field`, `Modal`.
  The independent test-writing agent found two real bugs here too: a
  `prefix` prop collision with the native RDFa `prefix` attribute in
  `Field`'s select mode, and a shorthand/longhand CSS mix warning in the
  focus-ring style - both fixed before merge.
- [#10](https://github.com/zudaR107/schloss-ui/pull/20) `StatTile`,
  `Amount` (real inconsistency fixed: kuvert's Budget "Available"
  column already colors by sign, account balances and transaction
  amounts elsewhere didn't), `Sparkline`.
- [#11](https://github.com/zudaR107/schloss-ui/pull/21) `Toast` - a
  genuinely new pattern, not a fix.
- [#12](https://github.com/zudaR107/schloss-ui/pull/22) icon rules
  documented in README.md, `ICON_SIZE` exported.

Tagged `v0.1.0`, published to GitHub Packages as `@zudar107/schloss-ui`.
Consumer adoption (schloss#56, schlussel#59, kuvert#82/#83/#84) is next.

## schloss: adopted schloss-ui (2026-07-14)

First consumer adoption issue done - swapped schloss's local Header,
Footer, and ThemeToggle's hand-styled button for `@zudar107/schloss-ui`'s
Header/Footer/Button/Badge, replaced the hand-copied token file with an
`@import` of the package's `tokens.css`, layered schloss's own purple
accent on top, and fixed the favicon's stroke width (2.4 -> 2) to match
the shared icon rules. Existing tests (`HomePage.test.tsx` etc.) kept
passing unchanged against the new import source, confirming a clean
match per the issue's own signal check - no new test-writing agent
dispatch needed. Merged:
[schloss#67](https://github.com/zudaR107/schloss/pull/67).

Wiring GitHub Packages auth (a scoped registry, requiring auth even for
public packages) into CI and the Docker build surfaced two follow-ups:

- **Local dev auth**: pnpm refuses to expand env vars in a *project*
  `.npmrc`'s auth line (blocks a malicious committed `.npmrc` from
  exfiltrating a token to an attacker registry) - so regenerating
  `pnpm-lock.yaml` against the real `^0.1.0` registry dependency needed
  a one-time local `pnpm config set "//npm.pkg.github.com/:_authToken"
  "$(gh auth token)" --location user` (writes to the user's own
  `~/.npmrc`, not the repo) run by the user themselves.
- **Docker build bug** (schloss#68,
  [PR#69](https://github.com/zudaR107/schloss/pull/69)): the first
  Publish run on `main` after #67 failed -
  `[ERR_PNPM_MINIMUM_RELEASE_AGE_VIOLATION]` for the freshly-published
  `schloss-ui@0.1.0`. Root cause: `pnpm-workspace.yaml` (holding the
  `minimumReleaseAgeExclude` entry pnpm itself generated for the new
  package) wasn't in the Dockerfile's `COPY` list, so the install step
  inside the container never saw the exclusion. Fixed by adding it to
  `COPY`; confirmed the very next Publish run on `main` succeeded.

CHANGELOG entry: [schloss#70](https://github.com/zudaR107/schloss/pull/70).
Next: schlussel#59, kuvert#82/#83/#84 - expect the same local-dev-auth
and (if any consumer's Dockerfile also lacks `pnpm-workspace.yaml` in its
`COPY` list) the same Docker build fix to recur.

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
