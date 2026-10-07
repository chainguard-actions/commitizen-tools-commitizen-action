<!-- markdownlint-disable -->

# Hardening Report: commitizen-tools--commitizen-action/0.27.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **commitizen-tools--commitizen-action/0.27.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (b) violation: The shell variable `$INPUT_WORKING_DIRECTORY` (which holds the value of `inputs.working_directory`, a workflow-controlled input) is expanded unquoted in the `cd` command on line 38 of entrypoint.sh: `cd $INPUT_WORKING_DIRECTORY`. An attacker-controlled value containing shell metacharacters (spaces, semicolons, command substitution, etc.) could alter the command's behavior. The variable must be double-quoted: `cd "$INPUT_WORKING_DIRECTORY"`.

Locations:

- `entrypoint.sh:38`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed unquoted variable expansion in entrypoint.sh line 38: changed `cd $INPUT_WORKING_DIRECTORY` to `cd "$INPUT_WORKING_DIRECTORY"`. This prevents shell metacharacter injection from the workflow-controlled `inputs.working_directory` value.

