<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--with-post-step/v8.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--with-post-step/v8.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

In main.js, the caller-controlled `key` input (read from `process.env.INPUT_KEY`) is written directly to `GITHUB_STATE` without newline sanitization. The value is uppercased but never passed through `tr -d '\n\r'` before being appended to the special environment file. An attacker-controlled `key` input containing embedded newlines could inject arbitrary key=value pairs into the GitHub Actions state file, potentially hijacking subsequent step outputs or environment variables.

Offending code:
  const key = process.env.INPUT_KEY.toUpperCase();
  appendFileSync(process.env.GITHUB_STATE, `${key}=true${EOL}`);

Fix: sanitize the key before writing, e.g.:
  const key = process.env.INPUT_KEY.toUpperCase().replace(/[\r\n]/g, '');

Locations:

- `main.js:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** github-env-injection

**Notes:**

Fixed github-env-injection in hardened/action/main.js at line 44. Added `.replace(/[\r\n]/g, '')` after `.toUpperCase()` when reading the INPUT_KEY environment variable. This strips any carriage return or newline characters from the caller-controlled `key` input before it is written to GITHUB_STATE, preventing injection of arbitrary key=value pairs into the GitHub Actions state file.

