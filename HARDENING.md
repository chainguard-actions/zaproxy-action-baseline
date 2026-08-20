<!-- markdownlint-disable -->

# Hardening Report: zaproxy--action-baseline/v0.12.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zaproxy--action-baseline/v0.12.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a different (potentially malicious) commit. Failing references:
- check-dist.yml: `actions/checkout@v4`, `actions/setup-node@v4`, `actions/upload-artifact@v3`
- check-run.yml: `actions/checkout@v4`
Each should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-dist.yml:22`
- `.github/workflows/check-dist.yml:25`
- `.github/workflows/check-dist.yml:43`
- `.github/workflows/check-run.yml:18`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` block, and neither job within them defines a job-level `permissions:` block. Without explicit permissions, workflows inherit the repository's default token permissions (which may be `write-all`), granting unnecessary access. A minimal permissions block (e.g. `permissions: read-all` or specific scopes) should be added to each workflow.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/check-run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

**check-dist.yml**:
- Pinned `actions/checkout@v4` → `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4`
- Pinned `actions/setup-node@v4` → `actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`
- Pinned `actions/upload-artifact@v3` → `actions/upload-artifact@ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5 # v3`
- Added top-level `permissions: {}` and job-level `permissions: contents: read`

**check-run.yml**:
- Pinned `actions/checkout@v4` → `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4`
- Added top-level `permissions: {}` and job-level `permissions: contents: read`

