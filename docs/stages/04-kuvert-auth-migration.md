# Stage 4 — Kuvert migrates to the centralized auth flow

**Repo:** kuvert · **Branch:** `feat/centralized-auth-migration` · **Status:** done
**Depends on:** Stage 3 merged first (needs a real schlussel login page to redirect to).

Merged: [kuvert#30](https://github.com/zudaR107/kuvert/pull/30)

`return_to` sent to schlussel always points at kuvert's own
`/auth/callback?next=<original path>` rather than the original path
directly — centralizes token handling (extract from hash, fetch
`/auth/me`, strip from history, hand off via client-side nav) in one route
instead of every protected route needing its own hash-checking logic.

Two real bugs surfaced only by actually running the full 4-container stack
(schlussel + schlussel-web + kuvert-api + kuvert-web) rather than reading
the config:
- nginx's `/auth/` proxy prefix swallowed the new `/auth/callback` SPA
  route and forwarded it straight to the schlussel API (404) — fixed with
  a more specific `location` block ahead of the general proxy.
- `kuvert/docker-compose.yml` defaulted `JWT_ISSUER` to `schloss` while
  schlussel always signs as `schlussel` (hardcoded in schlussel's own
  compose file) — every cross-service token would have failed validation
  on a real `docker compose up` without a manual override. Fixed the
  default; kuvert-api's own code-level default already had this right.

Verified: registered + logged in via schlussel-web, used the resulting
token directly against kuvert-api — 200, user auto-provisioned via JWKS.

## Plan

- Remove `kuvert/web/src/features/auth/LoginPage.tsx` and its `/login` route from
  `router/index.tsx`.
- Add an `/auth/callback` route:
  - Reads `location.hash` for the token.
  - Calls `setAccessToken(token)`.
  - Strips the hash via `history.replaceState` (don't leave the token sitting in the
    URL/browser history).
  - Navigates to the originally intended page, or `/budget` as a default.
- Update `router/index.tsx`'s `beforeLoad` guard on the `protectedLayout` route: when
  there's no access token, redirect via a **full browser navigation** (not client-side
  router nav — this needs to leave the app) to:
  `${SCHLUSSEL_URL}/login?return_to=${encodeURIComponent(currentUrl)}`
- Update `useAuth.ts`:
  - Remove the local `login()` function (no longer needed).
  - Keep `logout()` — but its redirect target changes from `/login` (removed) to
    schlussel's hosted login. Stage 0's fix already added the `navigate()` call after
    `logout()`; this stage updates *where* it points.
  - Keep the on-mount silent-refresh logic (`schluesselFetch('/refresh', ...)` — this
    already works today and needs no change).
- New env var: `VITE_SCHLUSSEL_URL` — add to `.env.example`.
- Replace the now-obsolete `LoginPage.test.tsx` with tests for:
  - The new `/auth/callback` route (token extraction, hash stripping, navigation).
  - The redirect-when-unauthenticated behavior (full navigation to schlussel, not a
    client-side route change).

## Tests

Per above — this stage removes an entire test file and needs new coverage for what
replaces it. Don't leave `LoginPage.test.tsx` for a component that no longer exists.

## Verification

- `pnpm test` in `kuvert/web` passes.
- Manual, end-to-end: log out of kuvert, confirm redirect to schlussel's real login
  page (not a 404), log in there, confirm redirect back to kuvert lands on the
  originally-intended page with a working session (refresh the page — should stay
  logged in via the silent-refresh flow, not schlussel's redirect flow).
