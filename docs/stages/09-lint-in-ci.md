# Stage 9 — Lint in CI

**Repos:** schlussel, schloss, kuvert (three separate branches/PRs) · **Status:** done

Merged: [schlussel#12](https://github.com/zudaR107/schlussel/pull/12) ·
[schloss#10](https://github.com/zudaR107/schloss/pull/10) ·
[kuvert#16](https://github.com/zudaR107/kuvert/pull/16)

kuvert's api had no linter at all before this - added oxlint there too
(not just wiring up web's existing one), cleaning up a handful of real
dead-code warnings (unused imports, an unused variable shadowed by a
later recomputation) found along the way in api and schlussel.

## Problem

schloss and kuvert/web already have `oxlint` as a devDependency and a `lint`
script, but it never runs in CI — only tests do (Stage 1). schlussel and
kuvert/api have no linter configured at all yet.

## Plan

- schloss: add a `lint` step to the existing `.github/workflows/test.yml`
  (`pnpm lint`).
- kuvert: add a `lint` step to the `web` job (`pnpm --filter web lint`).
  Check whether `api/` has (or should get) its own lint script - if not,
  decide whether to add oxlint there too as part of this stage, or split
  into a follow-up.
- schlussel: has no lint script or linter dependency at all. Add oxlint
  (matching the convention used elsewhere in the platform) and a `lint`
  step in its workflow.

## Tests

No new application tests - this wires up existing/newly-added lint
tooling into CI.

## Verification

Push each branch, confirm the lint step actually runs and would fail on
a deliberately introduced lint error (verify once, don't need to leave
the broken code in).
