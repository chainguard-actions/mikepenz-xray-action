<!-- markdownlint-disable -->

# Hardening Report: mikepenz--xray-action/v4.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mikepenz--xray-action/v4.1.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The `commit-dist` job's `run:` block directly interpolates GitHub Actions expressions inside a shell command string. The offending line is:

  git push "https://x-access-token:${{ secrets.RENOVATE_TOKEN }}@github.com/${{ github.repository }}.git" "HEAD:${{ github.event.pull_request.head.ref }}"

`${{ github.repository }}` and `${{ github.event.pull_request.head.ref }}` are attacker-controllable values (via pull request events) that are substituted directly into the shell command before the shell ever sees them. A malicious branch name in `github.event.pull_request.head.ref` could inject arbitrary shell commands. These values must be moved into `env:` variables and then double-quoted in the shell script.

Locations:

- `.github/workflows/build.yml:60`

### missing-permissions (severity: medium)

The workflow file `renovate.yml` has no top-level `permissions:` key, and its single job (`renovate`) also has no job-level `permissions:` key. Without explicit permissions, the job inherits the repository's default token permissions, which may be overly broad (e.g., `write` access to contents). A minimal `permissions:` block should be added.

Locations:

- `.github/workflows/renovate.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, missing-permissions

**Notes:**

1. build.yml (script-injection): Moved `${{ github.repository }}`, `${{ github.event.pull_request.head.ref }}`, and `${{ secrets.RENOVATE_TOKEN }}` out of the `run:` shell string in the 'commit-dist' job's 'Commit rebuilt dist' step into an `env:` block as GH_REPOSITORY, PR_HEAD_REF, and RENOVATE_TOKEN respectively. The shell script now references them as plain environment variables, eliminating the injection risk from attacker-controlled branch names. 2. renovate.yml (missing-permissions): Added `permissions: {}` at the top/workflow level to restrict the default GITHUB_TOKEN, and `permissions: contents: read` at the job level for the renovate job (minimum needed for checkout). Renovate uses its own RENOVATE_TOKEN secret for its operations.

