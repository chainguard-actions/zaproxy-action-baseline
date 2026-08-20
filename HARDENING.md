<!-- markdownlint-disable -->

# Hardening Report: zaproxy--action-baseline/v0.13.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zaproxy--action-baseline/v0.13.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files are pinned to mutable version tags instead of immutable 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the upstream action tag is moved to a different (potentially malicious) commit.

Offending references:
- `actions/checkout@v4` (check-dist.yml line 23, check-run.yml line 19)
- `actions/setup-node@v4` (check-dist.yml line 26)
- `actions/upload-artifact@v3` (check-dist.yml line 44)

Each should be replaced with a full SHA pin, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-dist.yml:23`
- `.github/workflows/check-dist.yml:26`
- `.github/workflows/check-dist.yml:44`
- `.github/workflows/check-run.yml:19`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and neither job within them declares a job-level `permissions:` block. Without explicit permissions, GitHub Actions grants the default token permissions (which can be write-all on some repository configurations), violating the principle of least privilege. A minimal `permissions: {}` or specific scopes (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/check-run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files (.github/workflows/check-dist.yml and .github/workflows/check-run.yml):

1. **unpinned-uses**: Pinned all four action references to immutable full commit SHAs:
   - `actions/checkout@v4` → `@11d5960a326750d5838078e36cf38b85af677262 # v4` (both files)
   - `actions/setup-node@v4` → `@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4` (check-dist.yml)
   - `actions/upload-artifact@v3` → `@ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5 # v3` (check-dist.yml)

2. **missing-permissions**: Added `permissions: {}` at the top-level of both workflow files (deny-all default), and `permissions: contents: read` at the job level (minimum required for checkout).

