<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--releaser/v4.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--releaser/v4.2.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is interpolated directly inside a run: shell command string. In composite/action.yml line 52, the run: block contains `run: '''${{ github.action_path }}/../releaser.py'''`. Any ${{ ... }} expression directly inside a run: script is a script-injection finding — the value flows through YAML template substitution before the shell ever sees it, bypassing shell quoting. The fix is to use the $GITHUB_ACTION_PATH environment variable instead: `run: '''$GITHUB_ACTION_PATH/../releaser.py'''`.

Locations:

- `composite/action.yml:52`

### unpinned-uses (severity: high)

The root action.yml uses a Docker action with a mutable image reference that has no tag and no SHA digest: `image: 'docker://ghcr.io/pytooling/releaser'`. This is vulnerable to supply-chain attacks because the image can be replaced at any time. The image reference must be pinned to a specific SHA digest, e.g. `image: 'docker://ghcr.io/pytooling/releaser@sha256:<64-hex-char-digest>'`.

Locations:

- `action.yml:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

1. Fixed script injection in composite/action.yml line 52: replaced `${{ github.action_path }}` with `$GITHUB_ACTION_PATH` (the built-in GitHub Actions environment variable), eliminating YAML template substitution before the shell sees the value. 2. Fixed unpinned Docker image in action.yml line 44: pinned `docker://ghcr.io/pytooling/releaser` to `docker://ghcr.io/pytooling/releaser:latest@sha256:6137404904edef5409e084b88e1ab33f52db02aa39ac2c5f488a768337555099` using the actual digest from the registry, preserving the `docker://` scheme.

