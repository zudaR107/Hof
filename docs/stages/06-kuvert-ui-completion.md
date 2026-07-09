# Stage 6 — Kuvert UI completion

**Repo:** kuvert · **Status:** done

Merged, in order: [kuvert#33](https://github.com/zudaR107/kuvert/pull/33) (Modal
primitive) · [kuvert#34](https://github.com/zudaR107/kuvert/pull/34) (Accounts) ·
[kuvert#35](https://github.com/zudaR107/kuvert/pull/35) (Debts) ·
[kuvert#36](https://github.com/zudaR107/kuvert/pull/36) (Transactions) ·
[kuvert#37](https://github.com/zudaR107/kuvert/pull/37) (wire up create-period/
create-goal/contribute buttons) · [kuvert#38](https://github.com/zudaR107/kuvert/pull/38)
(Settings page + new `GET`/`PUT /users/me` endpoint, closes #6).

Six sub-PRs against the same issue, exactly as this file's outline suggested. Allocating
budget to an envelope turned out to already be wired (inline click-to-edit in
`BudgetPage.tsx`), so only period creation itself was actually dead there.

Every page here was built with **tests written by a fresh subagent from a behavioral
spec, never given the implementation source** — the corrected standing-workflow rule
(see ROADMAP.md). Real bugs these agents' tests caught, found and fixed without the
agent ever reading the implementation:
- `DebtsPage`'s due-date field had `min={today()}`, which silently blocks native form
  submission (HTML5 constraint validation) when editing a debt whose existing due date
  is already in the past — a legitimate, common case. Removed the constraint.
- A stale compiled `dist/` directory in `kuvert/api` was shadowing the real test files
  (vitest discovered both), and a test-order-dependent leftover `currency` value in
  shared seed data affected the new `users.test.ts`. Neither was an implementation bug;
  fixed the test infrastructure (added `dist/` to vitest's exclude, reset `currency` in
  the shared `cleanDb()` helper) instead.
- Two of the agents' own tests had authoring bugs (an aria-label button matched by empty
  `textContent`, and a grammatically-wrong Russian modal title) — caught, diagnosed, and
  fixed directly, without touching the implementation.

No visual/browser verification was possible in this environment — component tests plus
`build`/`lint` were the practical ceiling for the pure-UI pieces. The Settings page's new
API endpoint was covered by real Hono-app integration tests (same pattern as every other
router), not just unit tests.

## Problem (confirmed by exploration)

- Transactions, Accounts, and Debts pages currently render "In development"
  (`router/index.tsx`'s `Placeholder` component) despite the API fully supporting all
  three (`GET/POST/PUT/DELETE` already implemented in
  `kuvert/api/src/features/{transactions,accounts,debts}/router.ts`).
- Every "create new" button across `BudgetPage.tsx` and `GoalsPage.tsx` has no
  `onClick` handler at all — dead buttons. There is no Modal/Dialog component anywhere
  in the codebase yet.
- No Settings page exists. `users.currency` is a real DB column (default `'RUB'`) that
  is never surfaced or editable anywhere in the UI.

## Scope for this stage (to be broken into sub-branches/PRs, likely one per bullet)

- A reusable Modal/Dialog primitive — the first one needed by any of the below.
- Real Transactions page: list with filters (the API already supports `accountId`,
  `envelopeId`, `type`, `from`/`to`, `limit`/`offset` query params), create/edit modal.
- Real Accounts page: list, create/edit, balance display (`GET /:id/balance` already
  implemented).
- Real Debts page: list with `settled` filter, create/edit, mark-settled action.
- Wire up the dead buttons: create period, allocate to envelope/category (BudgetPage),
  create goal, contribute to goal (GoalsPage).
- Settings page: currency at minimum.

## When this stage starts

Re-derive the exact plan from the current state of the code at that time — this file
is an outline, not a frozen spec. Check whether Stages 0-5 changed any relevant
patterns (e.g. the auth flow) that this UI work should follow.
