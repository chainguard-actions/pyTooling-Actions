<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--releaser/v4.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--releaser/v4.2.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a `run:` shell command string. In `composite/action.yml` line 52, the run block is: `run: '''${{ github.action_path }}/../releaser.py'''`. The `${{ github.action_path }}` expression is substituted by the YAML template engine before the shell executes the string, bypassing shell quoting. Any `${{ ... }}` directly inside a `run:` block is a script-injection risk regardless of which context it reads from.

Locations:

- `composite/action.yml:52`

### unpinned-uses (severity: high)

The Docker action in `action.yml` references a mutable image without a SHA digest: `image: 'docker://ghcr.io/pytooling/releaser'`. This image has no tag (defaults to `latest`) and no `@sha256:<digest>` pin, making it vulnerable to supply-chain attacks where the image could be silently replaced. It must be pinned to a full SHA256 digest, e.g. `docker://ghcr.io/pytooling/releaser@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:47`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

1. composite/action.yml (line 52): Moved `${{ github.action_path }}` out of the `run:` block into the step's `env:` block as `ACTION_PATH`. The run command now uses the plain shell variable `"$ACTION_PATH/../releaser.py"`. The existing env vars (INPUT_TOKEN, INPUT_FILES, etc.) were merged into the same single `env:` block. 2. action.yml (line 47): Pinned the Docker image from `docker://ghcr.io/pytooling/releaser` (untagged/mutable) to `docker://ghcr.io/pytooling/releaser:latest@sha256:6137404904edef5409e084b88e1ab33f52db02aa39ac2c5f488a768337555099`, preserving the `docker://` scheme and adding the `:latest` tag inline with the digest.

