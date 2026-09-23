<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--with-post-step/v8.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--with-post-step/v8.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

In main.js, the value of `inputs.key` (a caller-controlled input) is read from `process.env.INPUT_KEY`, uppercased into the variable `key`, and then written directly to the `GITHUB_STATE` special environment file via `appendFileSync(process.env.GITHUB_STATE, \`${key}=true${EOL}\`)` without any newline sanitization. An attacker controlling the `key` input can inject newlines to write arbitrary key=value pairs into GITHUB_STATE, potentially hijacking the post-step detection logic or poisoning state for downstream steps. The required sanitization (equivalent of `printf '%s' "$key" | tr -d '\n\r'`) is absent before the write.

Locations:

- `main.js:39`
- `main.js:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** github-env-injection

**Notes:**

Fixed github-env-injection in hardened/action/main.js by adding `.replace(/[\r\n]/g, '')` after `.toUpperCase()` when deriving the `key` variable from `process.env.INPUT_KEY`. This strips all newline (\n) and carriage return (\r) characters from the caller-controlled input before it is written to the GITHUB_STATE file, preventing an attacker from injecting arbitrary key=value pairs into the state file.

