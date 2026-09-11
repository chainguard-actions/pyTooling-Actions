<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--with-post-step/v8.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--with-post-step/v8.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

In main.js, the value of `inputs.key` (a caller-controlled input) is read as `process.env.INPUT_KEY`, transformed with `.toUpperCase()`, and then written directly to the special GitHub environment file `GITHUB_STATE` via `appendFileSync(process.env.GITHUB_STATE, \`${key}=true${EOL}\`)` without any newline sanitization (i.e., no `printf '%s' ... | tr -d '\n\r'` equivalent). An attacker who controls the `key` input could inject newlines to write arbitrary key-value pairs into the runner's state file, potentially influencing subsequent steps. The fix is to strip newline characters from `key` before writing it to `GITHUB_STATE`.

Locations:

- `main.js:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** github-env-injection

**Notes:**

In hardened/action/main.js, added `.replace(/[\r\n]/g, '')` to the `key` variable assignment (line 44) to strip carriage return and newline characters before writing to GITHUB_STATE. This prevents an attacker who controls the `key` input from injecting newlines to write arbitrary key-value pairs into the runner's state file.

