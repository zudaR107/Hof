# Stage 7 — Budget domain logic

**Repo:** kuvert (api) · **Status:** done

Merged: [kuvert#39](https://github.com/zudaR107/kuvert/pull/39) (rollover) ·
[kuvert#40](https://github.com/zudaR107/kuvert/pull/40) (recurring goals) ·
[kuvert#41](https://github.com/zudaR107/kuvert/pull/41) (CSV import, closes #7).

All three pieces confirmed the doc's own instinct: lazy computation on read,
no cron, no explicit "close period"/"start new cycle" action anywhere.

- **Rollover**: computed the first time anyone reads or allocates into the
  next period, then persisted so it isn't recomputed every time. Chains
  compound correctly across any number of un-viewed periods, since each
  period's own `carriedOver` is computed the same way from whatever came
  before it. A previous period only counts as closed once its `endDate`
  has passed; overspending clamps to zero, never negative.
- **Recurring goals**: every `GET /goals` call checks for completed
  recurring goals and, for each, archives the old cycle and creates a
  fresh one (same name/icon/color/target/recurrence, `currentAmount` reset
  to 0) in the same request. `recurringDay` optionally gates the earliest
  day of the month a new cycle may start.
- **CSV import**: asked the user before designing anything, per this
  file's own instruction — confirmed a generic/universal format was
  wanted, not one tied to a specific bank. `POST /transactions/import`
  accepts `{ accountId, csv }`; header row needs `date`/`amount`/`type`,
  optional `note`/`envelope` (matched by name, never auto-created).
  Invalid rows are skipped and reported rather than failing the whole
  import. No CSV library - a small RFC 4180-ish parser in
  `api/src/utils/csv.ts` covers quoted fields and embedded commas. No
  deduplication yet (documented limitation, not solved speculatively).

Tests for all three were written by independent subagents from behavioral
specs, without reading the implementations. One real bug slipped through
review by coincidence of timing but was actually a **test-authorship**
issue, not an implementation one: a rollover test allocated an older
period *after* a newer one had already been allocated, which correctly
(and intentionally) caused the newer period to inherit the older one's
carryover at PUT-time — the test's expected value didn't account for that
compounding. Fixed by reordering the test's setup, not the implementation.
No actual implementation bugs were found across all three pieces.

## Problem (confirmed by exploration)

Three schema fields are currently inert — present in the DB, validated by Zod schemas,
never actually acted on by any business logic:

- **Rollover**: `envelopes.rolloverEnabled` is stored but never read anywhere outside
  its own Zod schema. `envelopeBudgets.carriedOver` is hardcoded to `0` at creation
  (`periods/router.ts`) and only ever read back, never recalculated. There is no
  period-close logic anywhere — no endpoint or job computes a nonzero `carriedOver` for
  a subsequent period.
- **Recurring goals**: `goals.recurring` / `goals.recurringDay` are stored but nothing
  reads them to regenerate a goal cycle after completion.
- **CSV import**: `transactions.importId` exists per its own code comment ("for future
  CSV import tracking") but is always `null` — no import endpoint exists.

## Scope for this stage

Each of the three above is substantial enough to likely become its own sub-stage file
(and its own branch/PR) when reached:

- Rollover-on-period-close: needs a design decision on *when* this runs (an explicit
  "close period" action the user triggers, vs. computed lazily whenever the next
  period's budget is first viewed). Lazy computation avoids needing a scheduler/cron at
  all — worth strongly preferring for a self-hosted single-service app.
- Recurring-goal regeneration: similarly, prefer computing this lazily on next relevant
  read/write rather than introducing a background job/cron dependency, if at all
  practical.
- CSV import: needs a format decision (which columns, which bank/export formats to
  target — this is genuinely open, revisit with the user before designing).

## When this stage starts

Don't assume the lazy-computation approach without confirming it still fits the
architecture at that point — re-check the codebase and, for CSV import specifically,
ask the user what input format(s) they actually need before designing anything.
