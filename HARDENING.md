<!-- markdownlint-disable -->

# Hardening Report: commitizen-tools--commitizen-action/0.27.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **commitizen-tools--commitizen-action/0.27.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

entrypoint.sh writes version strings obtained from `cz version --project` (which reads repository-controlled version files — attacker-controllable in pull-request scenarios) directly to $GITHUB_ENV and $GITHUB_OUTPUT without the required `printf '%s' ... | tr -d '\n\r'` sanitization. A malicious version string containing newlines could inject arbitrary environment variables or outputs into subsequent workflow steps. Affected writes: PREVIOUS_REVISION/previous_version (lines 38–39), PREVIOUS_REVISION_MAJOR/previous_version_major (lines 42–43), PREVIOUS_REVISION_MINOR/previous_version_minor (lines 45–46), REVISION/version/next_version (lines 100–102), NEXT_REVISION_MAJOR/next_version_major (lines 105–106), NEXT_REVISION_MINOR/next_version_minor (lines 108–109).

Locations:

- `entrypoint.sh:38`
- `entrypoint.sh:39`
- `entrypoint.sh:42`
- `entrypoint.sh:43`
- `entrypoint.sh:45`
- `entrypoint.sh:46`
- `entrypoint.sh:100`
- `entrypoint.sh:101`
- `entrypoint.sh:102`
- `entrypoint.sh:105`
- `entrypoint.sh:106`
- `entrypoint.sh:108`
- `entrypoint.sh:109`

### script-injection (severity: high)

Rule (b) violation: `cd $INPUT_WORKING_DIRECTORY` on line 35 of entrypoint.sh uses an unquoted shell variable expansion of `INPUT_WORKING_DIRECTORY`, which is a workflow-controlled input (`inputs.working_directory`) inherited as a process environment variable. An attacker-supplied value containing shell metacharacters (spaces, globs, etc.) could alter the command's behaviour. The variable must be double-quoted: `cd "$INPUT_WORKING_DIRECTORY"`.

Locations:

- `entrypoint.sh:35`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, github-env-injection

**Notes:**

Fixed two findings in hardened/action/entrypoint.sh: (1) script-injection: quoted $INPUT_WORKING_DIRECTORY in the `cd` command (`cd "$INPUT_WORKING_DIRECTORY"`); (2) github-env-injection: added a `sanitize()` helper function using `printf '%s' "$1" | tr -d '\n\r'` and wrapped all six `cz version --project` captures (PREV_REV, PREV_REV_MAJOR, PREV_REV_MINOR, REV, NEXT_REV_MAJOR, NEXT_REV_MINOR) with it before writing to $GITHUB_ENV and $GITHUB_OUTPUT.

