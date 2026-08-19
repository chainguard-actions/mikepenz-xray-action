<!-- markdownlint-disable -->

# Hardening Report: mikepenz--xray-action/v4.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mikepenz--xray-action/v4.0.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in build.yml are pinned to mutable version tags instead of full 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if those tags are moved. Failing references: `actions/checkout@v6` (lines 14, 36, 53), `actions/setup-node@v6` (line 15), `mikepenz/release-changelog-builder-action@v6` (line 57), `mikepenz/action-gh-release@v2` (line 62).

Locations:

- `.github/workflows/build.yml:14`
- `.github/workflows/build.yml:15`
- `.github/workflows/build.yml:36`
- `.github/workflows/build.yml:53`
- `.github/workflows/build.yml:57`
- `.github/workflows/build.yml:62`

### unpinned-uses (severity: high)

Multiple `uses:` references in codeql-analysis.yml are pinned to mutable version tags instead of full 40-character commit SHAs. Failing references: `actions/checkout@v6` (line 35), `github/codeql-action/init@v4` (line 39), `github/codeql-action/autobuild@v4` (line 47), `github/codeql-action/analyze@v4` (line 57).

Locations:

- `.github/workflows/codeql-analysis.yml:35`
- `.github/workflows/codeql-analysis.yml:39`
- `.github/workflows/codeql-analysis.yml:47`
- `.github/workflows/codeql-analysis.yml:57`

### missing-permissions (severity: medium)

build.yml has no top-level `permissions:` key and none of its three jobs (build, test, release) define job-level `permissions:`. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (write access to contents, packages, etc.).

Locations:

- `.github/workflows/build.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all findings across both workflow files:

**build.yml**:
- Pinned `actions/checkout@v6` → `@df4cb1c069e1874edd31b4311f1884172cec0e10 # v6` (3 occurrences)
- Pinned `actions/setup-node@v6` → `@249970729cb0ef3589644e2896645e5dc5ba9c38 # v6`
- Pinned `mikepenz/release-changelog-builder-action@v6` → `@c9bcd8238b6f41e05561348339429d360b1c0247 # v6`
- Pinned `mikepenz/action-gh-release@v2` → `@5c3d16ffbdc3e0fbfe2c8a69a448798f5d9b30c2 # v2`
- Added top-level `permissions: contents: write` (minimum needed for release job)
- Added job-level `permissions: contents: read` for `build` and `test` jobs (least privilege)
- Added job-level `permissions: contents: write` for `release` job (needed to create GitHub releases)

**codeql-analysis.yml**:
- Pinned `actions/checkout@v6` → `@df4cb1c069e1874edd31b4311f1884172cec0e10 # v6`
- Pinned `github/codeql-action/init@v4` → `@7188fc363630916deb702c7fdcf4e481b751f97a # v4`
- Pinned `github/codeql-action/autobuild@v4` → `@7188fc363630916deb702c7fdcf4e481b751f97a # v4`
- Pinned `github/codeql-action/analyze@v4` → `@7188fc363630916deb702c7fdcf4e481b751f97a # v4`
- (Already had job-level permissions block, no changes needed there)

