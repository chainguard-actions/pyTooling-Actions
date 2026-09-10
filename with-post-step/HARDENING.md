<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--with-post-step/v4.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--with-post-step/v4.2.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

In main.js, the value of `INPUT_KEY` (the `key` action input) is read, uppercased, and written unsanitized into the `GITHUB_STATE` special environment file: `appendFileSync(process.env.GITHUB_STATE, \`${key}=true${EOL}\`)`. Because no newline-stripping sanitization (`printf '%s' ... | tr -d '\n\r'`) is applied before the write, a caller-controlled `key` input containing embedded newlines can inject additional arbitrary key=value pairs into `GITHUB_STATE`, potentially overwriting or spoofing state variables used by subsequent steps or the post step.

Locations:

- `main.js:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** github-env-injection

**Notes:**

Fixed github-env-injection in hardened/action/main.js: added `.replace(/[\r\n]/g, '')` to strip newline characters from `INPUT_KEY` before it is uppercased and written to `GITHUB_STATE`. This prevents a caller-controlled `key` input with embedded newlines from injecting additional arbitrary key=value pairs into the GITHUB_STATE special environment file.

