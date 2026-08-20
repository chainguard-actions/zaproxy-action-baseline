<!-- markdownlint-disable -->

# Hardening Report: zaproxy--action-baseline/v0.15.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zaproxy--action-baseline/v0.15.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files use mutable tag-based refs (@v4) instead of immutable 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the referenced action tag is moved or compromised. Affected references: `actions/checkout@v4`, `actions/setup-node@v4`, `actions/upload-artifact@v4` in check-dist.yml; `actions/checkout@v4` in check-run.yml.

Locations:

- `.github/workflows/check-dist.yml:23`
- `.github/workflows/check-dist.yml:26`
- `.github/workflows/check-dist.yml:44`
- `.github/workflows/check-run.yml:19`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` key, and no job within either file defines a job-level `permissions:` key. Without explicit permissions, workflows run with the default (potentially broad) token permissions. Minimal required permissions should be declared explicitly (e.g., `permissions: read-all` or specific scopes).

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/check-run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all action references to full 40-char SHAs with tag comments preserved — actions/checkout@v4→@11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v4→@49933ea5288caeca8642d1e84afbd3f7d6820020, actions/upload-artifact@v4→@ea165f8d65b6e75b540449e92b4886f43607fa02; (2) Added top-level `permissions: contents: read` to both check-dist.yml and check-run.yml, granting only the minimum permissions needed for checkout operations.

