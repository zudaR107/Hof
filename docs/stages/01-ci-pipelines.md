# Stage 1 — CI pipelines

**Repos:** schlussel, schloss, kuvert (three separate branches/PRs) · **Status:** done

Merged: [schlussel#5](https://github.com/zudaR107/schlussel/pull/5) ·
[schloss#5](https://github.com/zudaR107/schloss/pull/5) ·
[kuvert#9](https://github.com/zudaR107/kuvert/pull/9)

kuvert's workflow runs `api` and `web` test suites as two separate jobs
(125 + 74 tests). schloss's `test` script is `tsc -b --noEmit` since it
has no real test suite yet — a genuine typecheck, not a faked pass.

## Problem

schlussel and kuvert both have real vitest test suites (302 tests total as of the last
count) that only ever run locally/manually — no CI runs them on push or PR. schloss has
no test script at all yet.

## Plan

For each repo, add `.github/workflows/test.yml`:
- Trigger: `push` and `pull_request`.
- Steps: checkout, setup Node (match the version already used, check each repo's
  `package.json`/`.nvmrc` if present), install pnpm, `pnpm install`, `pnpm test`.
- **kuvert** is a pnpm workspace with two test suites (`api/` and `web/`) — either run
  `pnpm -r test` at the workspace root, or two separate jobs (api, web) for clearer
  failure isolation. Prefer two jobs if it doesn't meaningfully slow things down.
- **schloss** has no `test` script yet. Add a minimal one before wiring CI to it — even
  `tsc --noEmit` as a placeholder typecheck is fine, but don't claim a green check runs
  tests that don't exist. Note explicitly in the PR description that schloss's "test"
  step is a typecheck only, pending real tests (tracked separately, not blocking this
  stage).

## Branch names

- schlussel: `chore/ci-pipeline`
- schloss: `chore/ci-pipeline`
- kuvert: `chore/ci-pipeline`

## Tests

No new application tests — this stage is the delivery mechanism for existing ones.

## Verification

- Push the branch, open a draft PR (or just check the Actions tab after push) and
  confirm the workflow actually runs and goes green in each repo.
