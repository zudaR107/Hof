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

## schlussel: adopted schloss-ui (2026-07-14)

Second consumer adoption issue done - Header/Footer, the name/email/
password form fields, and the submit buttons on Login/RegisterPage now
use `@zudar107/schloss-ui`. Surfaced a real gap in the package itself:
`Field` only had a decorative left-side `prefix` slot, but schlussel's
password inputs need an interactive show/hide toggle on the right -
added a symmetric, pointer-events-enabled `suffix` slot rather than
bolting on a fragile pixel-offset workaround
([schloss-ui#23](https://github.com/zudaR107/schloss-ui/issues/23),
merged as [PR#24](https://github.com/zudaR107/schloss-ui/pull/24),
tagged `v0.2.0`, tests written by an independent subagent - 6 new
cases, no mismatches).

One existing test needed a real update, not a mechanical one: the
shared Header intentionally drops the visible "Schlüssel" text label
next to the logo (the logo icon is itself the home link now, brand
identity conveyed via a title tooltip instead) - `headerFooter.test.tsx`
was asserting on that removed text content, updated to assert on the
`title` attribute instead. Every other existing test kept passing
unchanged. Merged: [schlussel#60](https://github.com/zudaR107/schlussel/pull/60).

Hit a second, repo-specific Docker build bug this rollout's own
`.npmrc`/CI wiring didn't cover: this repo is a pnpm workspace (API at
root, `web/`), and the root API image's `Dockerfile` (which never uses
schloss-ui at all) still failed on the same `[ERR_PNPM_FETCH_401]` -
`pnpm install --frozen-lockfile` fetches every package in the lockfile
to verify it, not just the current project's own deps, even with
`--filter` (confirmed locally: `--filter` only scopes what gets linked
into `node_modules`, not what gets fetched). Fixed by adding the same
BuildKit-secret `.npmrc` auth to the root Dockerfile too
([schlussel#61](https://github.com/zudaR107/schlussel/issues/61),
[PR#62](https://github.com/zudaR107/schlussel/pull/62)).

Next: kuvert#82/#83/#84 - the largest of the three consumers, split
across three issues.

## kuvert: adopted schloss-ui, all three issues (2026-07-14)

Third and final consumer, the largest surface of the three - split
into three issues/PRs as planned, all merged same day, all with CI
green and existing tests kept passing unchanged throughout (245-247
tests depending on the batch).

- **Header/Footer/EmptyState/accent**
  ([kuvert#82](https://github.com/zudaR107/kuvert/pull/98)): mobile
  menu trigger moved into `Header`'s `leftSlot` (built for exactly this
  case); settings goes through `onSettings`/`useNavigate` instead of a
  router `Link`; the user's name is now a single-initial avatar instead
  of visible text. Emoji-based empty states across Budget, Accounts,
  Goals, Debts, and Transactions replaced with the shared `EmptyState` -
  kept two non-actionable cases (closed-debts tab, "add an account
  first") as local elements since the shared component always requires
  an action button. Switched to kuvert's own teal accent (`#0d9488`),
  fixed the logo's stroke width (2.2 -> 2). One existing test needed a
  real update (avatar/title instead of visible name; button instead of
  link for settings) for the same reasons as schlussel's Header test.
- **Buttons/badges/filters/numbers**
  ([kuvert#83](https://github.com/zudaR107/kuvert/pull/99)): every ad
  hoc button now uses the shared `Button`; Debts' Active/Closed filter
  is now a `SegmentedControl`; transaction types, debt status, and a
  goal's "Достигнуто" pill are now `Badge`s; account balances, Budget's
  Available column, debt amounts, and income/expense transaction rows
  use `Amount`'s sign-based coloring (transfers keep their own
  info-blue text - `Amount` only models gain/loss/neutral). Added
  `StatTile` summary strips to Goals/Debts/Transactions. Two bullets
  from the original issue didn't apply to what was actually built:
  schloss-ui never shipped a "Card" component (the existing card icon
  treatment already matched the target pattern), and "Скоро" is
  schloss's own placeholder pill, not kuvert's.
- **Form fields/modals/toasts**
  ([kuvert#84](https://github.com/zudaR107/kuvert/pull/100)): every
  create/edit form's inputs now use the shared `Field`; the local
  `Modal` component is replaced by the shared one everywhere and
  deleted outright (each form's submit button moved from inside the
  form to the Modal's footer, triggered via the form's `id` +
  `requestSubmit()`); the shared `Toast` gets its first real usage
  anywhere on the platform, via a small new `useToast` hook, wired into
  every create/update/archive/settle/delete flow across all five
  pages. Tests for `useToast` and the first page's toast wiring (new
  capability, not a swap) written by an independent subagent from a
  behavioral spec - 11 new tests, no bugs found.

All three PRs' Docker builds succeeded on first merge (kuvert's
Dockerfiles already copied `pnpm-workspace.yaml` into their build
context from the start, so neither of the two Docker-auth bugs found
in schloss/schlussel recurred here).

**This closes out the entire schloss-ui rollout**: tokens, all 12
components, and all three consumer repos (schloss, schlussel, kuvert)
now share one design system instead of hand-copied, drifting styles.
The published design-review artifact
(https://claude.ai/code/artifact/9074a748-5b32-4187-964e-0503e6ceeca4)
now reflects real, shipped code rather than a proposal - still needs a
manual follow-up to delete or repurpose it, no tool available to do
that automatically.

## docker-compose.yml missing the npm_token build secret (2026-07-14)

Found while preparing local/docker-compose test instructions after the
rollout above: each of schloss, schlussel, and kuvert's Dockerfiles
mount a BuildKit secret (`npm_token`) to authenticate to GitHub
Packages, wired into CI's `docker/build-push-action` step - but never
into `docker-compose.yml` itself. A plain `docker compose build`/
`up --build` (local dev, or a real docker-compose-based production
deploy) failed with `ERR_PNPM_FETCH_401`. Fixed by declaring the
secret in all three repos' `docker-compose.yml`, sourced from a
`NODE_AUTH_TOKEN` environment variable set by whoever runs the build.
Confirmed `docker compose config` still validates with and without
the token set, and that tor's CI check (which validates the full
multi-repo compose config with no token available) is unaffected.
Merged: [schloss#72](https://github.com/zudaR107/schloss/pull/72),
[schlussel#67](https://github.com/zudaR107/schlussel/pull/67),
[kuvert#102](https://github.com/zudaR107/kuvert/pull/102).

## Docker builds failing on pnpm's deps-status check (2026-07-14)

Found on the user's first real local `docker compose up --build`,
resolved in two passes.

**First pass** (wrong diagnosis): every Dockerfile installed pnpm
unpinned (`corepack prepare pnpm@latest` or `npm install -g pnpm`),
and the build failed with
`[ERR_PNPM_ABORTED_REMOVE_MODULES_DIR_NO_TTY]` on pnpm 11.13.0. Looked
like the same class of issue as the pnpm 11.12.0 self-installer bug
pinned around in CI weeks earlier
([kuvert#80](https://github.com/zudaR107/kuvert/pull/80) and its
follow-ups) - just never applied to the Dockerfiles themselves. Pinned
all five Dockerfiles (schloss, schlussel x2, kuvert x2) to the same
`11.7.0` CI already uses. Merged:
[schloss#74](https://github.com/zudaR107/schloss/pull/74),
[schlussel#69](https://github.com/zudaR107/schlussel/pull/69),
[kuvert#104](https://github.com/zudaR107/kuvert/pull/104).

**Second pass** (the real fix): the pin alone didn't work - the same
error reproduced on 11.7.0 too, on the user's next build attempt.
pnpm runs a deps-status check before any `run`/`exec` script and, on a
mismatch, tries to reinstall - which needs interactive confirmation to
purge `node_modules`, and a Docker build has no TTY to give it,
regardless of pnpm version. The actual reason CI never hit this: GitHub
Actions sets `CI=true` for every workflow automatically, which is
exactly pnpm's own suggested remedy for this situation. Added
`ENV CI=true` explicitly to each Dockerfile's builder stage, before the
script that triggers the check. Merged:
[schloss#76](https://github.com/zudaR107/schloss/pull/76),
[schlussel#71](https://github.com/zudaR107/schlussel/pull/71),
[kuvert#106](https://github.com/zudaR107/kuvert/pull/106).

## Real hands-on testing batch, first live run behind tor (2026-07-14)

The user's first real walkthrough of the whole platform after the
docker-compose build finally succeeded, surfacing five issues:

- **Field's left padding too small everywhere**
  ([schloss-ui#25](https://github.com/zudaR107/schloss-ui/issues/25),
  [PR#26](https://github.com/zudaR107/schloss-ui/pull/26), tagged
  `v0.3.0`): the same shorthand/longhand style-mixing bug class
  already fixed once for `border`/`borderColor` - `fieldBoxStyle`'s
  `padding` shorthand was mixed with conditionally-set
  `paddingLeft`/`paddingRight` longhands. jsdom's style engine doesn't
  reproduce the browser's exact cascade resolution here, so the
  existing 20-test Field suite never caught it - only visible in a
  real browser. Fixed by never mixing shorthand and longhand padding.
- **Debts/Transactions empty-state icons: missing background, rendered
  flush left instead of centered** (kuvert-only - schloss/schlussel
  don't have this pattern): Tailwind's preflight sets
  `svg { display: block }`, which
  breaks `text-align: center` centering for the two ad hoc empty-state
  fallbacks that don't fit the shared `EmptyState` component (unlike
  `EmptyState` itself, which centers via an explicit flex wrapper, not
  text-align). Wrapped both in the same tinted, centered badge
  treatment as everywhere else.
- **Header's home link showed the current service's own icon instead
  of schloss's**: the link leads to schloss (schlussel/kuvert have no
  home page of their own), but the badge showed schlussel's key /
  kuvert's envelope icon instead of schloss's logo - confusing, since
  it looked like the current service's own identity rather than
  signaling "this goes to a different app". Fixed in both to show
  schloss's own logo mark.
- **Header logout button appeared not to work** (kuvert only): could
  not conclusively reproduce the exact mechanism via code review alone
  (no browser automation tooling available in this environment) -
  added proper error handling and a toast on failure as a hardening
  measure, which should at least surface the real cause the next time
  it's tested, rather than failing silently.
- **Footer should show the running version**: added an optional
  `version` prop to the shared `Footer` (schloss-ui v0.3.0, same PR as
  the Field fix above), wired to each app's own package.json version
  via a Vite `define` (schlussel/kuvert read their root package.json,
  since `web/`'s own version was never bumped past the Vite scaffold
  default "0.0.0"; schloss's own root version was bumped from
  "0.0.0" to "0.1.0" to match its siblings). Both `vite.config.ts` and
  `vitest.config.ts` needed the `define` in all three repos, since
  they're separate configs that don't share one.

Merged: [schloss#78](https://github.com/zudaR107/schloss/pull/78),
[schlussel#73](https://github.com/zudaR107/schlussel/pull/73),
[kuvert#108](https://github.com/zudaR107/kuvert/pull/108). All three
Docker builds succeeded on `main` after merge.

## Real hands-on testing, round two: a genuine cross-origin logout bug (2026-07-14)

The user's second walkthrough (after the fixes above) surfaced a real,
platform-wide auth bug, plus a footer content request.

**Logout never actually worked, on schloss or kuvert, since the SSO
silent-reauth architecture was adopted** - not a schloss-ui regression,
a pre-existing bug this round of testing happened to finally exercise
directly. Root cause: the session cookie is host-only to schlussel's
own origin (no `Domain` attribute, by design - see the earlier "SSO
correction" entry above for why). Both apps' `logout()` called
schlussel's `/auth/logout` via a `fetch` proxied through their *own*
origin - from the browser's point of view that's a same-origin request
to schloss/kuvert, not schlussel, so the cookie was never attached,
the DB session row was never deleted, and the "clear cookie" response
targeted the wrong origin too. The subsequent redirect to schlussel's
login page then silently re-authenticated the user via the
still-valid session (the existing, correctly-working silent-reauth
mechanism) - bouncing straight back to where they started, which is
exactly what "logout does nothing" looks like.

Presented two fix options to the user (real browser navigation vs.
loosening CORS/SameSite for a direct cross-origin call); they picked
the former, matching the platform's existing silent-reauth pattern
rather than trading away cookie security posture for convenience.

- **schlussel**: new `/logout` page ([#74](https://github.com/zudaR107/schlussel/issues/74),
  [PR#75](https://github.com/zudaR107/schlussel/pull/75)) - does the
  real logout same-origin via a normal browser navigation, then
  bounces back to `return_to`. Merged and deployed first, since
  schloss/kuvert's fixes depend on it existing.
- **schloss** ([#79](https://github.com/zudaR107/schloss/issues/79),
  [PR#80](https://github.com/zudaR107/schloss/pull/80)) and **kuvert**
  ([#109](https://github.com/zudaR107/kuvert/issues/109),
  [PR#110](https://github.com/zudaR107/kuvert/pull/110)): `logout()`
  now only clears local state; a new `buildSchluesselLogoutUrl()`
  helper (mirroring the existing `buildSchluesselLoginUrl`, but
  synchronous - no PKCE needed for logout) sends the browser to
  schlussel's `/logout` instead. Two of kuvert's existing tests needed
  real updates (asserting the new `/logout` redirect target instead of
  the old, broken `/login` one) - a genuine behavior change, not a
  mechanical fix.

**Footer description**: `Footer` gained an optional `description` prop
(schloss-ui [#27](https://github.com/zudaR107/schloss-ui/issues/27),
[PR#28](https://github.com/zudaR107/schloss-ui/pull/28), tagged
`v0.4.0`) - one short sentence per service, rendered as its own line
above the existing tagline (kept that tagline text exactly as it was,
so nothing already querying the combined string broke). Wired into all
three consumers: schloss "домашняя страница и точка входа", schlussel
"вход и аккаунты", kuvert "конвертное бюджетирование". Merged:
[schloss#82](https://github.com/zudaR107/schloss/pull/82),
[schlussel#77](https://github.com/zudaR107/schlussel/pull/77),
[kuvert#112](https://github.com/zudaR107/kuvert/pull/112).

Also confirmed with the user that the Debts empty-state icon color
difference (accent-tinted "Active" tab, which has a real CTA, vs.
muted "Closed" tab, which doesn't) is intentional, not a bug -
deliberately left as-is pending any further feedback.

## GitHub process rules formalized (2026-07-15)

The user gave a standing checklist for how issues/PRs/milestones/the
Project board/branches should be run from here on - written into the
"Standing workflow" section above. The key change from how this session
had been operating: **milestone = one umbrella task made of several
issues**, and **one branch per milestone** (not per issue as before);
PR titles now follow `type(module): head`; issues need this repo's real
custom labels; the Project board's Status field must be kept current,
not just populated once.

## kuvert: keep-previous-data on tab/filter/period switches (2026-07-15)

First stage run under the new process rules above - a fresh milestone
([kuvert#22](https://github.com/zudaR107/kuvert/milestone/22) "Avoid
loading flicker on query-key switches"), three issues, one shared
branch, one PR.

The user reported the Debts page's Активные/Закрытые toggle flashing a
loading skeleton for a moment on every switch - the exact same class of
flicker fixed for cross-page navigation earlier (see "kuvert: remove
tab-switch flash via route loaders" above), but this time within a
single page: the `useQuery` key includes the toggled filter, so
TanStack Query treats each switch as a brand-new, uncached query and
`isLoading` flips true. A code audit while fixing it found the
identical anti-pattern in two more places: Budget's period-navigation
arrows and Transactions' filter controls.

Fix: `placeholderData: keepPreviousData` on all three `useQuery` calls -
the previous list stays on screen (instead of being cleared to a
skeleton) until the new data actually arrives. Filed as
[kuvert#113](https://github.com/zudaR107/kuvert/issues/113) (Debts,
the reported bug), [#114](https://github.com/zudaR107/kuvert/issues/114)
(Budget), [#115](https://github.com/zudaR107/kuvert/issues/115)
(Transactions), all closed by
[PR#116](https://github.com/zudaR107/kuvert/pull/116). This is now a
standing pattern: any `useQuery` keyed on local UI state (a filter, a
tab, a selected period) gets `placeholderData: keepPreviousData`.

## Logout v2: the real root cause (2026-07-15)

The user reported "Выйти" still not working even after the previous
fix (schlussel's same-origin `/logout` page), with a Network-tab
screenshot showing `POST auth.localhost/logout` succeed, immediately
followed by `POST kuvert.localhost/refresh` also succeeding - the user
was silently still logged in.

**Root cause**: every consumer app (kuvert, schloss) proxies `/auth/*`
straight to `schlussel:4000` from its own Caddy (docker) or vite dev
proxy, transparently to the browser. The session cookie has no
`Domain` attribute (host-only, by design), so a `Set-Cookie` on a
response to one of these proxied calls gets scoped by the browser to
that consumer app's own origin (`kuvert.localhost`), not
`auth.localhost` - a second, fully independent session cookie
schlussel's own same-origin `/logout` page can never see or clear.
This cookie is minted the moment a consumer app exchanges its PKCE
code at `/auth/token` (proxied), and kept alive indefinitely by that
app's own on-mount `/auth/refresh` polling.

Presented two fix layers via `AskUserQuestion`; the user picked the
combined, recommended option over a logout-only patch:

- **Root fix** ([schlussel#79](https://github.com/zudaR107/schlussel/issues/79),
  [PR#80](https://github.com/zudaR107/schlussel/pull/80)): `/login`'s
  PKCE branch, `/token`, and `/refresh` now only call `establishSession`
  (i.e. only emit `Set-Cookie`) when the request carries a trusted
  header (`X-Schlussel-Frontend: 1`) that only schlussel/web's own
  Caddy/vite proxy injects - consumer apps' own proxies never add it,
  so their proxied calls stop minting a cookie at all going forward.
  `/logout`'s cookie-clearing stays unconditional. Six pre-existing
  tests that implicitly assumed unconditional cookie issuance were
  updated to either add the trusted header (representing schlussel's
  own genuine usage) or assert the new, correct untrusted default (no
  cookie) where they represented a consumer app's call.
- **Immediate patch** ([kuvert#118](https://github.com/zudaR107/kuvert/issues/118)/[PR#119](https://github.com/zudaR107/kuvert/pull/119),
  [schloss#84](https://github.com/zudaR107/schloss/issues/84)/[PR#85](https://github.com/zudaR107/schloss/pull/85)):
  the root fix alone doesn't clear a cookie a user's browser already
  holds from before it shipped - `logout()` in both apps now also
  calls its own proxied `/auth/logout` first, immediately clearing that
  app's own cookie and its server-side row, before navigating to
  schlussel's `/logout` page for the `auth.localhost`-scoped one.

## Footer polish: capitalized descriptions + nicer styling (2026-07-15)

Two small pieces of user feedback on the Footer `description` line
added the previous round: it needed a capital letter, and to look more
deliberately designed. schloss-ui `v0.4.1`
([#29](https://github.com/zudaR107/schloss-ui/issues/29),
[PR#30](https://github.com/zudaR107/schloss-ui/pull/30)) gave the line
a small accent-colored marker dot and a touch more weight/size; all
three consumers bumped and capitalized their own description string:
[schloss#83](https://github.com/zudaR107/schloss/issues/83)/[PR#86](https://github.com/zudaR107/schloss/pull/86),
[schlussel#78](https://github.com/zudaR107/schlussel/issues/78)/[PR#81](https://github.com/zudaR107/schlussel/pull/81),
[kuvert#117](https://github.com/zudaR107/kuvert/issues/117)/[PR#120](https://github.com/zudaR107/kuvert/pull/120).

## Docker build: intermittent better-sqlite3 compile failure (2026-07-15)

The user reported `docker compose build` occasionally failing with
`gyp ERR! find Python`. Root cause: better-sqlite3's install script
downloads a prebuilt binary for the target platform and only falls
back to compiling from source via node-gyp (which needs Python and a
C/C++ toolchain) if that download fails - `node:22-alpine` has
neither, so a transient network timeout fetching the prebuilt binary
(seen live: `prebuild-install warn install Request timed out`) turned
into a hard build failure instead of a slower but working fallback.
Hits both api images (which use better-sqlite3 directly) and web
images (which don't, but `pnpm install --frozen-lockfile` verifies/
builds every package in the shared workspace lockfile regardless of
`--filter`, a constraint already hit before).

Fix ([schlussel#82](https://github.com/zudaR107/schlussel/issues/82)/[PR#83](https://github.com/zudaR107/schlussel/pull/83),
[kuvert#121](https://github.com/zudaR107/kuvert/issues/121)/[PR#122](https://github.com/zudaR107/kuvert/pull/122)):
install `python3 make g++` in every builder/runner stage that runs a
frozen-lockfile install. Verified by reproducing the exact failure
(`npm install --build-from-source` in a bare `node:22-alpine`
container) and confirming it's resolved once the toolchain is present
- a full `docker compose build` couldn't be run locally (no GitHub
Packages token in this environment), so CI's build/push workflow was
the real end-to-end check.

## schloss: logout still broken, a distinct pre-existing bug (2026-07-15)

The user reported "выход с главной страницы" (logout from schloss's
home page) not working, right after the cross-origin-cookie root cause
was already fixed and verified for kuvert - a fair "didn't we already
fix this?" question, but this turned out to be a genuinely separate
bug in schloss's own `HomePage.tsx`, never exercised by earlier testing
(which only clicked logout on kuvert).

**Root cause**: `HomePage` has a `useEffect` watching `[loading, user]`
that redirects to schlussel's LOGIN page whenever `!loading && !user` -
written for a fresh, unauthenticated page load. `logout()` also clears
`user` to `null` on its way to navigating to schlussel's LOGOUT page,
which satisfies that same condition - the two navigations raced, and
the login redirect could win, silently re-authenticating the user via
a still-valid session and making it look exactly like logout did
nothing. Existing tests never caught this because they mock `useAuth()`
with a static value and never exercise a real `logout()`-driven `user`
transition.

Fixed ([#87](https://github.com/zudaR107/schloss/issues/87),
[PR#88](https://github.com/zudaR107/schloss/pull/88)) with a ref set
synchronously in the click handler, before `logout()`'s async work
starts, that the redirect effect checks. New regression test uses the
real `useAuthProvider` hook (not the existing file's mocked one) to
actually exercise the race - deterministic across 5 repeated runs.

## Standing workflow (every stage)

- **Milestone = one global/umbrella task**, made up of several issues (not
  one milestone per issue). Always create or reuse one before filing issues
  for a new batch of work.
- Issues get a properly written title (never containing the word "Stage")
  and this repo's own custom labels (`type:feat`/`type:fix`/`type:chore`/
  `type:test`/`type:docs`, not GitHub's stock ones) — check the repo's real
  label set (`gh label list`) before assigning, don't guess.
- Every issue and PR gets added to the GitHub Project board (project id 2,
  `PVT_kwHOBhrZh84BctmC`) with its Status field kept current (Todo → In
  Progress → Done) as work actually progresses, not just added and left.
- **One branch per milestone** (not per issue) — all issues under the same
  milestone are implemented together on that one shared branch, prefixed by
  type: `fix/...`, `feat/...`, `chore/...`, `test/...`. One PR from that
  branch into `main` per repo (a milestone touching multiple repos gets one
  branch + one PR per repo, since issues are per-repo).
- **PR title** follows Conventional Commits: `type(module): head` — same
  style as commit subject lines (English, imperative, ASCII-only).
- Claude creates the branch, implements the issue(s), then **dispatches a
  fresh subagent with no prior context to write the tests** — briefed with
  a behavioral spec (function signatures, expected behavior) but explicitly
  told not to read the implementation files, so the tests are unbiased and
  can actually catch bugs rather than mirroring whatever the code happens
  to do. Claude runs the resulting tests against the real implementation,
  fixes any real bugs they surface, then commits (GPG-signed, English,
  50/72 format, ASCII-only, **no Co-Authored-By or other self-attribution
  line, ever**).
- Claude pushes, opens the PR with **`Closes #N`** for every issue it
  closes (so they auto-close on merge), labeled, milestoned, assigned to
  `@me`, added to the Project board, and **merges it via `gh pr merge`
  once checks are clean** — no PR is left for the user to push/merge
  manually.
- Every stage that changes behavior gets tests before being considered done.
- After merge: delete the remote branch, clean up the local branch, update
  this file's status and the stage's own doc, set the Project board items
  to Done.
- Every PR also appends one brief bullet to the repo's own `CHANGELOG.md`
  (under whichever theme section fits, or a new section if none does) —
  not a full commit-by-commit log, just enough to see what shipped.

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
