<!-- markdownlint-disable -->

# Hardening Report: zaproxy--action-baseline/v0.14.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zaproxy--action-baseline/v0.14.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files are pinned to mutable version tags (@v4) rather than full 40-character commit SHAs. This exposes the action to supply-chain attacks if the upstream action tag is moved or compromised. Affected references:
- `.github/workflows/check-dist.yml`: `actions/checkout@v4`, `actions/setup-node@v4`, `actions/upload-artifact@v4`
- `.github/workflows/check-run.yml`: `actions/checkout@v4`

Locations:

- `.github/workflows/check-dist.yml:22`
- `.github/workflows/check-dist.yml:25`
- `.github/workflows/check-dist.yml:40`
- `.github/workflows/check-run.yml:13`

### missing-permissions (severity: medium)

Neither `.github/workflows/check-dist.yml` nor `.github/workflows/check-run.yml` declares a top-level `permissions:` key, and no job within either file has a `permissions:` block. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/check-run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. `.github/workflows/check-dist.yml`:
   - Added `permissions: {}` at the top level
   - Pinned `actions/checkout@v4` → `@11d5960a326750d5838078e36cf38b85af677262 # v4`
   - Pinned `actions/setup-node@v4` → `@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`
   - Pinned `actions/upload-artifact@v4` → `@ea165f8d65b6e75b540449e92b4886f43607fa02 # v4`

2. `.github/workflows/check-run.yml`:
   - Added `permissions: {}` at the top level
   - Pinned `actions/checkout@v4` → `@11d5960a326750d5838078e36cf38b85af677262 # v4`

All SHAs were resolved using lookup_action_sha against the live GitHub refs.

