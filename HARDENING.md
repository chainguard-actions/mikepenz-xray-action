<!-- markdownlint-disable -->

# Hardening Report: mikepenz--xray-action/v4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mikepenz--xray-action/v4.0.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in build.yml are pinned to mutable tags instead of full 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the tag is moved. Failing references: `actions/checkout@v6` (lines 14, 35, 54), `actions/setup-node@v6` (line 15), `mikepenz/release-changelog-builder-action@v6` (line 58), `mikepenz/action-gh-release@v2` (line 64).

Locations:

- `.github/workflows/build.yml:14`
- `.github/workflows/build.yml:15`
- `.github/workflows/build.yml:35`
- `.github/workflows/build.yml:54`
- `.github/workflows/build.yml:58`
- `.github/workflows/build.yml:64`

### unpinned-uses (severity: high)

Multiple `uses:` references in codeql-analysis.yml are pinned to mutable tags instead of full 40-character commit SHAs. Failing references: `actions/checkout@v6` (line 28), `github/codeql-action/init@v3` (line 32), `github/codeql-action/autobuild@v3` (line 38), `github/codeql-action/analyze@v3` (line 44).

Locations:

- `.github/workflows/codeql-analysis.yml:28`
- `.github/workflows/codeql-analysis.yml:32`
- `.github/workflows/codeql-analysis.yml:38`
- `.github/workflows/codeql-analysis.yml:44`

### missing-permissions (severity: medium)

build.yml has no top-level `permissions:` key and none of its three jobs (`build`, `test`, `release`) define a job-level `permissions:` block. Without explicit permissions, the workflow inherits the default broad token permissions, which violates the principle of least privilege.

Locations:

- `.github/workflows/build.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all findings across both workflow files:

**build.yml**:
- Pinned `actions/checkout@v6` to SHA `d23441a48e516b6c34aea4fa41551a30e30af803` (3 occurrences)
- Pinned `actions/setup-node@v6` to SHA `249970729cb0ef3589644e2896645e5dc5ba9c38`
- Pinned `mikepenz/release-changelog-builder-action@v6` to SHA `c9bcd8238b6f41e05561348339429d360b1c0247`
- Pinned `mikepenz/action-gh-release@v2` to SHA `5c3d16ffbdc3e0fbfe2c8a69a448798f5d9b30c2`
- Added top-level `permissions: {}` to deny all by default
- Added `permissions: { contents: read }` to `build` and `test` jobs
- Added `permissions: { contents: write }` to `release` job (required for creating GitHub releases)

**codeql-analysis.yml**:
- Pinned `actions/checkout@v6` to SHA `d23441a48e516b6c34aea4fa41551a30e30af803`
- Pinned `github/codeql-action/init@v3` to SHA `08d09a53f0f5d694f253bd25732e4429c9e9337f`
- Pinned `github/codeql-action/autobuild@v3` to SHA `08d09a53f0f5d694f253bd25732e4429c9e9337f`
- Pinned `github/codeql-action/analyze@v3` to SHA `08d09a53f0f5d694f253bd25732e4429c9e9337f`
- The codeql-analysis.yml already had a job-level permissions block, so no permissions changes were needed there

