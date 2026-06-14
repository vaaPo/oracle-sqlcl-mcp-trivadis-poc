# AI Guidance For SQLcl MCP Trivadis Iterations

## Goal

Make Trivadis guideline feedback part of each SQL and PL/SQL iteration done through SQLcl MCP.

## Required Session Setup

1. Connect with the intended saved SQLcl connection.
2. Fetch schema information before generating or changing SQL.
3. Run `set codescan on` in the SQLcl session.
4. Execute SQL or PL/SQL scripts with `@file.sql` to surface inline warnings.

## Minimal MCP Workflow

1. `sqlcl_connect` using the saved connection name.
2. `sqlcl_schema_information` for the active schema.
3. `sqlcl_sqlcl_run` with `set codescan on`.
4. `sqlcl_sqlcl_run` with `@/absolute/path/to/script.sql`.
5. Read and act on any `SQL best practice warning` lines before accepting the script.

## Automatic Improvement Policy

The agent should fix warnings automatically when all of these are true:

- The warning has a clear low-risk fix.
- The fix does not change business intent.
- The fix does not require product or domain judgment.

Examples:

- Replace `select *` with explicit column lists.
- Remove obviously unused variables.
- Apply simple mechanical formatting or labeling changes.

## Ask-The-User Policy

The agent should ask before changing code when any of these are true:

- Multiple valid rewrites exist.
- The warning touches naming, public interfaces, or team conventions.
- The fix may affect behavior, performance, privileges, or deployment order.
- The warning indicates a deeper design issue rather than a local style issue.

## Recording Policy

1. Log each warning in `trivadis_warning_status.md` using one row per source file and Trivadis rule.
2. Update detection versions when the warning persists across iterations.
3. Add the fix version and fix stamp after the warning is resolved.
4. Keep source-file version comments aligned with the latest fix.

## Practical Rule

Do not assume `@file.sql` alone will show Trivadis warnings. Enable `set codescan on` first, or run `codescan -path ...` for broader explicit scans.
