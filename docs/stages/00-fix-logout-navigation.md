# Stage 0 — Fix logout navigation

**Repo:** kuvert · **Branch:** `fix/logout-navigation` (merged, deleted) · **Status:** done
**Issue:** [kuvert#2](https://github.com/zudaR107/kuvert/issues/2) · **PR:** [kuvert#1](https://github.com/zudaR107/kuvert/pull/1) (merged 2026-07-07)

## Problem

Clicking "logout" in the sidebar (`kuvert/web/src/components/Layout.tsx`) doesn't
redirect to `/login` immediately — the user has to manually refresh the page.

## Root cause (confirmed by code reading)

- `kuvert/web/src/lib/api.ts`: `accessToken` is a plain module-level `let` variable,
  not React state, not subscribed to by anything.
- `kuvert/web/src/router/index.tsx`: the `protectedLayout` route's `beforeLoad` checks
  `getAccessToken()` and throws `redirect({ to: '/login' })` if falsy. `beforeLoad` only
  re-runs on an actual navigation event or an explicit `router.invalidate()` call — never
  in reaction to that module variable changing.
- `kuvert/web/src/hooks/useAuth.ts` → `logout()` clears the token and sets `user` to
  `null`, but never triggers navigation.
- Contrast with `LoginPage.tsx`, which explicitly calls `navigate({ to: '/budget' })`
  inside a `useEffect` watching `user` — that's why login redirects immediately and
  logout doesn't.

## Fix

- In `Layout.tsx`, import `useNavigate` from `@tanstack/react-router`.
- Change the logout button's `onClick` to an async handler:
  ```tsx
  const navigate = useNavigate()
  // ...
  onClick={async () => { await logout(); navigate({ to: '/login' }) }}
  ```
- `@tanstack/react-router` is pinned at `^1.170.17` — this API is available.

## Tests

- Extend the existing test setup (same router-mocking pattern used in
  `LoginPage.test.tsx`) to add a Layout test asserting that clicking logout results in
  a navigation call to `/login`.

## Note for later

Stage 4 (kuvert auth migration) will change this redirect target from the local
`/login` route to schlussel's hosted login page, once that route is removed. This
stage's fix should still be a minimal, correct improvement on its own terms — not
gated on stage 4.

## Verification

- `pnpm test` in `kuvert/web` passes.
- Manual check: log in via `pnpm dev`, click logout, confirm immediate redirect
  without a manual refresh.
