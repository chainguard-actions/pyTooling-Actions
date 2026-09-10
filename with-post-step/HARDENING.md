<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--with-post-step/v8.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--with-post-step/v8.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

In main.js, the caller-controlled input `INPUT_KEY` is read, uppercased into `key`, and then written directly to the `GITHUB_STATE` special environment file via `appendFileSync(process.env.GITHUB_STATE, \`${key}=true${EOL}\`)` without any newline sanitization (i.e., without `printf '%s' ... | tr -d '\n\r'` or equivalent). Because `key` is derived from the `key` action input (which maps to `INPUT_KEY` and is set by the calling workflow), an attacker-controlled value containing newline characters could inject additional key=value pairs into `GITHUB_STATE`, potentially influencing the post-step detection logic or other state consumers. The required sanitization step is missing before the write.

Locations:

- `main.js:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** github-env-injection

**Notes:**

Fixed github-env-injection in hardened/action/main.js: Added `.replace(/[\r\n]/g, '')` after `.toUpperCase()` when deriving `key` from `INPUT_KEY`. This strips all carriage return and newline characters from the caller-controlled input before it is written to the GITHUB_STATE special environment file, preventing injection of additional key=value pairs via embedded newlines.

