<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--releaser/v4.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--releaser/v4.2.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command string in composite/action.yml. The offending line is: `run: '''${{ github.action_path }}/../releaser.py'''`. Any ${{ ... }} expression inside a run: block is a script-injection risk because YAML template substitution occurs before the shell ever sees the value, bypassing shell quoting. The fix is to use the pre-set environment variable $GITHUB_ACTION_PATH instead: `run: '''$GITHUB_ACTION_PATH/../releaser.py'''`.

Locations:

- `composite/action.yml:53`

### unpinned-uses (severity: high)

The Docker action in action.yml references a mutable image with no tag and no SHA digest: `image: 'docker://ghcr.io/pytooling/releaser'`. This image reference can be silently replaced at any time, enabling a supply-chain attack. It must be pinned to a specific SHA digest, e.g. `image: 'docker://ghcr.io/pytooling/releaser@sha256:<64-hex-char-digest>'`.

Locations:

- `action.yml:43`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

1. Fixed script injection in composite/action.yml (line 53): replaced `${{ github.action_path }}` with `$GITHUB_ACTION_PATH` to use the pre-set environment variable instead of YAML template substitution. 2. Fixed unpinned Docker image in action.yml (line 43): pinned `docker://ghcr.io/pytooling/releaser` to `docker://ghcr.io/pytooling/releaser@sha256:6137404904edef5409e084b88e1ab33f52db02aa39ac2c5f488a768337555099`, preserving the `docker://` scheme.

