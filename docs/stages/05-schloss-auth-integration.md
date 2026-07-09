# Stage 5 — Schloss home page auth integration

**Repo:** schloss · **Branch:** `feat/auth-integration` · **Status:** done
**Depends on:** Stage 3 (needs schlussel's hosted login page to exist).

Merged: [schloss#22](https://github.com/zudaR107/schloss/pull/22)

Decided during implementation: the home page stays fully public — it's a
launcher, not something worth hard-gating. Logged-out visitors see a
generic greeting and a "Войти" button; logged-in visitors see a
personalized greeting, their name, and a logout button. Reused the
kuvert Stage 4 pattern closely (`/auth/callback` route, `buildSchluesselLoginUrl`,
silent-refresh-on-mount `useAuth` hook).

Also added vitest + testing-library from scratch (schloss had no real
test framework before — CI only ran a `tsc -b --noEmit` typecheck
placeholder) and translated the remaining German UI text in `App.tsx`
(moved to `pages/HomePage.tsx`) and `ThemeToggle.tsx` to Russian — missed
during the earlier platform-wide translation pass.

**Methodology change starting this stage**: tests were written by an
independent subagent given only a behavioral spec (not the implementation
source), per corrected standing workflow — see ROADMAP.md. All 30 tests
passed against the real implementation on the first full run.

Verified with a real 3-container stack (schlussel + schlussel-web +
schloss): registered + logged in via schlussel-web, confirmed schloss's
nginx proxies `/auth/me` correctly and serves `/auth/callback` as the SPA
rather than proxying it (the same class of bug fixed in kuvert's Stage 4,
checked proactively here before it could bite again — motivated the
Stage 12 nginx-to-Caddy decision).

## Plan

- Show logged-in state in the header: user name, a logout button.
- Reuse the same silent-refresh-on-mount pattern already built in kuvert's
  `useAuth.ts` — worth writing down as a shared pattern (even without a shared npm
  package yet, since there's only two consumers so far) rather than reinventing the
  approach from scratch.
- Unauthenticated visitors get redirected to schlussel's login the same way kuvert
  does post-Stage-4 (full browser navigation to
  `${SCHLUSSEL_URL}/login?return_to=...`).
- Decide during implementation: does every page on schloss require auth, or just
  enough to show personalized content (user name, which service cards to show)? The
  home launcher may reasonably stay partially public — don't assume a hard gate
  without checking what actually makes sense for a single-user home page.

## Tests

- Component tests for logged-in vs logged-out header state.
- No test framework exists yet in schloss (confirmed during Stage 1 exploration) — if
  Stage 1 (CI pipelines) already added a minimal `test` script, build on that; if not,
  this stage needs to set up vitest + testing-library from scratch (same pattern as
  `kuvert/web`).

## Verification

- Manual: visit schloss logged out, confirm redirect behavior; log in, confirm user
  info displays; log out, confirm it clears immediately (this stage should not
  reintroduce the Stage-0-style logout bug — test for it explicitly).
