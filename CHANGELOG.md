# Changelog

Brief log of notable changes to this meta-repo — submodule bumps and
cross-cutting docs updates, not the services' own changes (see each
submodule's own `CHANGELOG.md` for that).

## Setup
- Initial repo: submodules for schlussel, schloss, kuvert, tor; README,
  LICENSE, and the existing ROADMAP.md/docs/stages carried over from the
  pre-git planning directory.

## Submodule bumps
- Bumped all four submodules to their open-source presentation polish
  commits (license/CI badges, per-repo GitHub descriptions and topics,
  gateway repo URL fixes after its rename to lowercase, and
  `.env.production.example` in tor).
- Bumped all four submodules again: the gateway's project name is now
  written lowercase ("tor") everywhere in prose, not just in URLs/slugs.
- Bumped schlussel/schloss/kuvert to their Authorization Code + PKCE
  commits - the login handoff no longer puts the access token in a URL.
  Verified live end-to-end against the real running stack.
- Bumped all four submodules to their community-health-files commits
  (CODE_OF_CONDUCT.md, SECURITY.md, issue/PR templates); added the same
  two files here, and removed `docs/stages/` now that all 13 original
  stages are done - the ROADMAP.md table plus each linked issue/PR already
  carries what was worth keeping.
- Bumped schlussel/schloss/kuvert again: their docker-compose.yml default
  origins (`ALLOWED_ORIGINS`, `VITE_ALLOWED_RETURN_ORIGINS`,
  `VITE_SCHLUSSEL_URL`, `VITE_KUVERT_URL`, `VITE_DEFAULT_APP_URL`) still
  assumed `http://`, but tor's gateway auto-upgrades everything to HTTPS -
  broke the return_to allowlist and CORS for anyone actually running the
  real stack. Found live by the user testing through the gateway.
- Bumped schlussel/kuvert/tor once more: `ALLOWED_ORIGINS` was the same
  outer variable name in both schlussel's and kuvert's docker-compose.yml,
  so tor's one shared `.env` fed the same value into both, silently
  overriding kuvert-api's own CORS allowlist. Split into
  `SCHLUSSEL_ALLOWED_ORIGINS`/`KUVERT_ALLOWED_ORIGINS`.
- Bumped schlussel/schloss/kuvert for the user-feedback batch: shared
  session across services (`COOKIE_DOMAIN`, schlussel), unified
  Header/Footer + a redrawn hero illustration/favicon (schloss), a
  header/footer on the login/register pages (schlussel/web), and a
  resizable sidebar + visible user identity + onboarding copy (kuvert).
- Bumped schlussel again: `COOKIE_DOMAIN` didn't actually work
  (`localhost` has no dot, so browsers won't share a Domain-scoped
  cookie across its subdomains) - reverted it and replaced with silent
  re-authentication through schlussel's own same-origin session.
  Verified live end-to-end against the real running stack.
- Bumped schlussel/schloss/kuvert once more: reduced the visible flicker
  during the first-time SSO redirect chain by restoring the stored theme
  before first paint on every page load, instead of flashing the
  default theme first.
- Bumped kuvert once more: switching between its own tabs also
  flickered - each page fetched its own data only after mounting.
  Fixed with TanStack Router loaders prefetching each route's data
  before the transition completes.
- Bumped kuvert again: replaced the sidebar's toggle button with
  click-anywhere-empty-to-toggle, and fixed the Footer being clipped
  and unreachable on tall pages (a missing `min-height: 0` on a flex
  item) - found live, in a fresh private window, ruling out caching.
- Bumped kuvert and tor once more: fixed the sidebar reverting a
  just-completed drag-resize (a synthetic click bubbling to the
  click-to-toggle handler); added a header with a link back to
  schloss's home page and settings/logout access (tor's new
  `SCHLOSS_URL` env var feeds it); and fixed a build-breaking unused
  import that `pnpm test` didn't catch but `docker compose build` did.
- Bumped kuvert/schlussel/schloss once more: shortened the Budget and
  Accounts empty-state copy to match every other tab's one-sentence
  hint; also pinned `pnpm/action-setup`'s version exactly in CI across
  all three - an unrelated pnpm 11.12.0 release broke every workflow
  run regardless of what changed.
- Added a fifth submodule, `schloss-ui` - a new repo for the shared
  design-token/component package the other three will consume. Design
  decided and seven issues filed (milestone "Shared UI system") before
  any implementation - see ROADMAP.md for the full design rationale.
- Reworked the schloss-ui plan after a deeper, iterative design pass
  (an HTML draft, refined across three rounds) - deleted the original
  seven issues outright and replaced them with 13 concrete ones
  covering Header, cards/empty states, buttons, badges, filters,
  forms, modals, icons, number coloring, and a new toast pattern.
- Bumped schloss-ui to `v0.1.0`: all 8 of its own issues from that plan
  shipped (tokens, publish pipeline, Header/Footer/EmptyState, Button/
  Badge/SegmentedControl, Field/Modal, StatTile/Amount/Sparkline,
  Toast, icon docs), plus one hover-feedback fix found along the way.
  Tagged and published to GitHub Packages - the three consumer
  adoption issues (schloss, schlussel, kuvert) are next.
- Bumped schloss to its schloss-ui adoption commit: local Header/Footer/
  ThemeToggle button and hand-copied tokens replaced with the shared
  package, schloss's own purple accent layered on top. Also fixed a
  Docker build bug the rollout surfaced - `pnpm-workspace.yaml` (holding
  pnpm's own `minimumReleaseAgeExclude` entry for the freshly-published
  package) wasn't copied into the build context, so the container-side
  install fell back to the default supply-chain policy and rejected the
  recently-published dependency. schlussel and kuvert are next, and may
  hit the same Docker fix if their Dockerfiles have the same gap.
- Bumped schloss-ui to `v0.2.0`: added a `suffix` slot to `Field`
  (symmetric to `prefix` but interactive), needed for schlussel's
  password show/hide toggle - a real capability gap found during
  consumer adoption, fixed in the package rather than worked around
  locally.
- Bumped schlussel to its schloss-ui adoption commit: Header/Footer,
  Login/RegisterPage's form fields, and the submit buttons now use the
  shared package. Hit a second, repo-specific Docker build bug: this
  repo is a pnpm workspace, and `pnpm install --frozen-lockfile`
  fetches every package in the lockfile to verify it regardless of
  `--filter`, so even the API image's Dockerfile (which never uses
  schloss-ui) needed the same GitHub Packages auth wired in. kuvert is
  next.
