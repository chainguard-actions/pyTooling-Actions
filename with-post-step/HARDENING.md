<!-- markdownlint-disable -->

# Hardening Report: pyTooling--Actions--with-post-step/v9.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pyTooling--Actions--with-post-step/v9.3.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

In main.js, the `inputs.key` value is read from `process.env.INPUT_KEY`, transformed with `.toUpperCase()` (which does NOT strip newlines), and then written directly into the GitHub Actions state file (`$GITHUB_STATE`) via `appendFileSync(process.env.GITHUB_STATE, \`${key}=true${EOL}\`)` without any newline sanitization. A caller supplying a `key` input containing embedded newline characters (e.g. `POST\nEVIL_VAR`) could inject additional `KEY=VALUE` pairs into the state file, poisoning state variables read by subsequent steps or the post step. The required sanitization (`key.replace(/[\r\n]/g, '')` or equivalent shell `tr -d '\n\r'`) is absent.

Locations:

- `main.js:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** github-env-injection

**Notes:**

Fixed github-env-injection vulnerability in hardened/action/main.js at line 44. The `key` variable derived from `process.env.INPUT_KEY.toUpperCase()` was written directly to `$GITHUB_STATE` without newline sanitization. Added `.replace(/[\r\n]/g, '')` to strip carriage return and newline characters from the key before it is written to the state file, preventing injection of additional KEY=VALUE pairs.

