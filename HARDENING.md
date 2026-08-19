<!-- markdownlint-disable -->

# Hardening Report: rowi1de--auto-assign-review-teams/v1.1.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **rowi1de--auto-assign-review-teams/v1.1.3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference third-party actions using mutable version tags instead of immutable full 40-character commit SHAs. This exposes the workflow to supply-chain attacks where a tag can be silently moved to point to malicious code. Failing references: auto-merge-dependabot.yaml — `alexwilson/enable-github-automerge-action@1.0.0` (lines 13 and 25); codeql-analysis.yml — `actions/checkout@v2` (line 36), `github/codeql-action/init@v1` (line 40), `github/codeql-action/autobuild@v1` (line 52), `github/codeql-action/analyze@v1` (line 63); test.yml — `actions/checkout@v1` (line 8).

Locations:

- `.github/workflows/auto-merge-dependabot.yaml:13`
- `.github/workflows/auto-merge-dependabot.yaml:25`
- `.github/workflows/codeql-analysis.yml:36`
- `.github/workflows/codeql-analysis.yml:40`
- `.github/workflows/codeql-analysis.yml:52`
- `.github/workflows/codeql-analysis.yml:63`
- `.github/workflows/test.yml:8`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` key, and no individual jobs define job-level `permissions:` blocks. Without explicit permissions, workflows inherit the default repository token permissions, which may be broader than necessary. This is especially risky for `auto-merge-dependabot.yaml`, which uses the `pull_request_target` trigger — a trigger that runs in the context of the base branch and has access to secrets even from fork PRs.

Locations:

- `.github/workflows/auto-merge-dependabot.yaml:1`
- `.github/workflows/codeql-analysis.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references by replacing mutable tags with full 40-character commit SHAs (preserving tags in comments). Added top-level permissions blocks to all three workflow files: auto-merge-dependabot.yaml gets contents:write and pull-requests:write; codeql-analysis.yml gets actions:read, contents:read, and security-events:write; test.yml gets contents:read and pull-requests:write.

