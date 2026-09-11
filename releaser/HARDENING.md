<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--releaser/v4.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--releaser/v4.2.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml uses a Docker image reference `docker://ghcr.io/pytooling/releaser` with no tag and no SHA digest. This is a mutable reference — the image can be silently replaced with a malicious version without any change to the action file. The image reference must be pinned to a specific SHA digest (e.g., `ghcr.io/pytooling/releaser@sha256:<64-hex-char-digest>`) to prevent supply-chain attacks.

Locations:

- `action.yml:47`

### script-injection (severity: high)

composite/action.yml contains a `run:` block that directly interpolates a `${{ ... }}` expression in the shell command string (sub-rule a): `run: '''${{ github.action_path }}/../releaser.py'''`. Any `${{ ... }}` expression interpolated directly inside a `run:` block is subject to YAML template substitution before the shell processes it, creating a script-injection risk. The value should be passed via an `env:` variable and referenced as `"$ACTION_PATH/../releaser.py"` instead.

Locations:

- `composite/action.yml:55`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. action.yml: Pinned the Docker image from `docker://ghcr.io/pytooling/releaser` (no tag, no digest) to `docker://ghcr.io/pytooling/releaser:latest@sha256:6137404904edef5409e084b88e1ab33f52db02aa39ac2c5f488a768337555099`, preserving the docker:// scheme and adding both the :latest tag and the immutable SHA digest. 2. composite/action.yml: Moved `${{ github.action_path }}` out of the `run:` block into the step's `env:` block as `ACTION_PATH`, and updated the shell command to reference `"$ACTION_PATH/../releaser.py"` instead. The existing INPUT_* env vars were merged into the same env block.

