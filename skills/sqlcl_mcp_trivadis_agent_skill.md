# SQLcl MCP Trivadis Agent Skill

## Purpose

Use this skill when an agent is generating, reviewing, or refactoring Oracle SQL or PL/SQL through SQLcl MCP and Trivadis guideline feedback should be part of the iteration.

## Trigger Conditions

- The user asks to create or modify Oracle SQL or PL/SQL.
- The user asks for SQL quality checks or Trivadis checks.
- The workflow uses SQLcl MCP for execution or validation.

## Required Behavior

1. Connect with the intended saved SQLcl connection.
2. Fetch schema information before writing or changing SQL.
3. Enable `set codescan on` before running any validation script.
4. Execute changed SQL using `@file.sql`.
5. Read inline Trivadis warnings from the execution output.
6. Fix low-risk warnings automatically.
7. Ask the user before applying higher-risk or judgment-based fixes.
8. Record warning lifecycle details in `trivadis_warning_status.md`.

## Low-Risk Fix Examples

- Replace `select *` with an explicit column list.
- Remove unused declarations.
- Apply simple mechanical improvements required by a rule.

## Escalation Cases

- Multiple valid rewrites exist.
- A warning touches naming or public interfaces.
- A fix may alter semantics, performance, privileges, or deployment order.

## Prompt Snippet

```text
Apply the SQLcl MCP Trivadis skill for this task. Connect with the saved SQLcl connection, fetch schema context, enable `set codescan on`, run the script with `@file.sql`, surface any Trivadis warnings, fix low-risk warnings automatically, and ask before making higher-risk changes.
```
