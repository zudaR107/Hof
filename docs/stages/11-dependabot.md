# Stage 11 — Dependabot

**Repos:** schlussel, schloss, kuvert (three separate branches/PRs) · **Status:** done

Merged: [schlussel#13](https://github.com/zudaR107/schlussel/pull/13) ·
[schloss#11](https://github.com/zudaR107/schloss/pull/11) ·
[kuvert#17](https://github.com/zudaR107/kuvert/pull/17)

kuvert needed two separate npm entries (`/api`, `/web`) since
Dependabot doesn't natively understand pnpm workspaces.

Additionally enabled Dependabot **security alerts** and **automated
security fixes** (repo settings, not code - via
`PUT /repos/{owner}/{repo}/vulnerability-alerts` and
`/automated-security-fixes`) in all three repos. This is a separate
GitHub feature from the version-update config above: it scans for
known CVEs in dependencies rather than just flagging new versions.

## Plan

Add `.github/dependabot.yml` to each repo:
- `package-ecosystem: npm`, directory `/` (and `/api`, `/web` for kuvert -
  Dependabot needs one entry per pnpm workspace package directory since it
  doesn't natively understand pnpm workspaces).
- `package-ecosystem: github-actions`, directory `/` - keeps the workflow
  actions themselves (`actions/checkout`, `pnpm/action-setup`, etc.) up to
  date too.
- Weekly schedule (avoid daily PR noise for a personal project).

## Tests

None - this is a GitHub-native feature, not application code. No branch
even required if GitHub allows committing this file directly, but follow
the same PR workflow for consistency and so CI validates the YAML runs
without error (Dependabot config errors show up in the repo's Insights >
Dependency graph > Dependabot tab, not as a CI failure - worth checking
that tab after merge, not just assuming success).

## Verification

After merge, check each repo's Insights > Dependabot tab to confirm the
config was accepted (no syntax errors) rather than assuming success from
a clean merge alone.
