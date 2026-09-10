<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--releaser/v4.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--releaser/v4.2.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression `${{ github.action_path }}` is directly interpolated inside a `run:` shell command string in composite/action.yml. The line `run: '''${{ github.action_path }}/../releaser.py'''` substitutes the expression value into the shell command before the shell ever sees it, enabling script injection if the value contains shell metacharacters. This should be replaced with the `$GITHUB_ACTION_PATH` environment variable instead.

Locations:

- `composite/action.yml:54`

### unpinned-uses (severity: high)

The Docker action in action.yml references `image: 'docker://ghcr.io/pytooling/releaser'` with no tag and no SHA digest (defaults to `latest`). This is a mutable image reference that can change at any time, exposing the action to supply-chain attacks. The image should be pinned to a specific SHA digest, e.g. `image: 'docker://ghcr.io/pytooling/releaser@sha256:<64-hex-char-digest>'`.

Locations:

- `action.yml:46`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

1. Fixed script-injection in composite/action.yml: replaced `${{ github.action_path }}` with the safe `$GITHUB_ACTION_PATH` environment variable in the run command. 2. Fixed unpinned-uses in action.yml: pinned the Docker image `ghcr.io/pytooling/releaser` from mutable `latest` to an immutable digest `docker://ghcr.io/pytooling/releaser:latest@sha256:6137404904edef5409e084b88e1ab33f52db02aa39ac2c5f488a768337555099`, preserving the docker:// scheme and tag inline.

