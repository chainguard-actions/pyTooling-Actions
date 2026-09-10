<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--with-post-step/v8.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--with-post-step/v8.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

In main.js, the value of `INPUT_KEY` (sourced from `inputs.key`, a caller-controlled action input) is written directly to the GitHub special state file (`GITHUB_STATE`) without newline sanitization. The line `appendFileSync(process.env.GITHUB_STATE, \`${key}=true${EOL}\`)` writes the unsanitized `key` variable (derived from `process.env.INPUT_KEY.toUpperCase()`) into the state file. A caller supplying a `key` value containing embedded newline characters (e.g. `FOO\nBAR`) could inject additional arbitrary `KEY=VALUE` entries into `GITHUB_STATE`, potentially overwriting or spoofing state variables consumed by the post step. The value must be sanitized (e.g. stripping `\n` and `\r`) before being written to any GitHub special environment file.

Locations:

- `main.js:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** github-env-injection

**Notes:**

In hardened/action/main.js, sanitized the `key` variable by adding `.replace(/[\r\n]/g, '')` before `.toUpperCase()` on the line where `INPUT_KEY` is read. This strips all carriage return and newline characters from the caller-controlled input before it is written to GITHUB_STATE, preventing newline injection attacks that could spoof or overwrite state variables consumed by the post step.

