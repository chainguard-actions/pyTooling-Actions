<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--with-post-step/v8.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--with-post-step/v8.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

In main.js, the value of `inputs.key` (accessed as `process.env.INPUT_KEY`) is uppercased and written directly to the `GITHUB_STATE` special environment file without newline sanitization. Specifically: `appendFileSync(process.env.GITHUB_STATE, \`${key}=true${EOL}\`)` where `key = process.env.INPUT_KEY.toUpperCase()`. A caller can supply a value for `inputs.key` containing embedded newline characters (e.g. `FOO=bar\nMALICIOUS`) to inject arbitrary additional entries into the runner's state file. The fix is to strip newlines before writing: `key.replace(/[\r\n]/g, '')` or equivalent sanitization before the `appendFileSync` call.

Locations:

- `main.js:46`

## Iteration Notes

### Iteration 1

**Fixes applied:** github-env-injection

**Notes:**

Fixed github-env-injection in hardened/action/main.js: Added `.replace(/[\r\n]/g, '')` to sanitize the `key` variable (derived from `process.env.INPUT_KEY.toUpperCase()`) before it is written to the GITHUB_STATE file via `appendFileSync`. This prevents an attacker from injecting arbitrary entries into the runner's state file by embedding newline characters in the `inputs.key` value.

