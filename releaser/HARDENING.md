<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--releaser/v4.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--releaser/v4.2.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The docker action in action.yml references the image 'docker://ghcr.io/pytooling/releaser' with no tag and no SHA digest. This is a fully mutable reference — any update to the image on the registry will silently change what code runs, enabling supply-chain attacks. The image must be pinned to a SHA digest, e.g. 'ghcr.io/pytooling/releaser@sha256:<64-hex-char-digest>'.

Locations:

- `action.yml:47`

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command string in composite/action.yml. The offending line is: `run: '''${{ github.action_path }}/../releaser.py'''`. Any ${{ ... }} expression interpolated directly into a run: block is a script-injection risk because the value is substituted into the shell command string before the shell parses it. Even though github.action_path is not directly attacker-controlled, the pattern is unsafe and must be replaced with an env: variable reference, e.g. set `ACTION_PATH: ${{ github.action_path }}` in the env: block and use `"$ACTION_PATH/../releaser.py"` in the run: script.

Locations:

- `composite/action.yml:55`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. action.yml: Pinned docker image from 'docker://ghcr.io/pytooling/releaser' (no tag, no digest) to 'docker://ghcr.io/pytooling/releaser:latest@sha256:6137404904edef5409e084b88e1ab33f52db02aa39ac2c5f488a768337555099', preserving the docker:// scheme and including the tag inline. 2. composite/action.yml: Moved ${{ github.action_path }} out of the run: shell command string into the step's env: block as ACTION_PATH, and updated the run: script to reference it as "$ACTION_PATH/../releaser.py" to eliminate the script-injection risk.

