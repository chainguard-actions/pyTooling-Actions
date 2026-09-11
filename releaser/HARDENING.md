<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--releaser/v4.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--releaser/v4.2.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression `${{ github.action_path }}` is directly interpolated inside a `run:` shell command string: `run: '''${{ github.action_path }}/../releaser.py'''`. Any `${{ ... }}` expression inside a `run:` block undergoes YAML template substitution before the shell sees it, making it a script-injection risk. The path should be passed via an `env:` variable and referenced as a quoted shell variable instead (e.g., `env: ACTION_PATH: ${{ github.action_path }}` then `run: "$ACTION_PATH/../releaser.py"`)

Locations:

- `composite/action.yml:52`

### unpinned-uses (severity: high)

The Docker action in `action.yml` references the image `docker://ghcr.io/pytooling/releaser` with no tag and no SHA digest. This is a fully mutable reference — any future push to the registry can silently change what code runs. The image must be pinned to an immutable SHA digest, e.g. `image: ghcr.io/pytooling/releaser@sha256:<64-hex-char-digest>`

Locations:

- `action.yml:45`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

1. composite/action.yml: Fixed script-injection by moving `${{ github.action_path }}` from the `run:` shell string into the `env:` block as `ACTION_PATH`. The run command now uses `"$ACTION_PATH/../releaser.py"` as a safe shell variable reference. The existing INPUT_* env vars were merged into the same env: block. 2. action.yml: Pinned the Docker image `docker://ghcr.io/pytooling/releaser` to its immutable SHA digest `docker://ghcr.io/pytooling/releaser:latest@sha256:6137404904edef5409e084b88e1ab33f52db02aa39ac2c5f488a768337555099`, preserving the `docker://` scheme required for Docker container actions.

