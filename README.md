# oracle-sqlcl-mcp-trivadis-poc

Public proof of concept showing how Oracle SQLcl Codescan and SQLcl MCP can be used to validate repository SQL or PL/SQL source files against Trivadis-style rules.

## Start Here

- Oracle SQLcl `CODESCAN` docs: <https://docs.oracle.com/en/database/oracle/sql-developer-command-line/26.1/sqcug/codescan.html>
- Trivadis PL/SQL and SQL Coding Guidelines: <https://trivadis.github.io/plsql-and-sql-coding-guidelines/>

## POC Scope

- Show that SQLcl can surface Trivadis warnings inline during script execution when `set codescan on` is enabled.
- Show how an MCP-based agent can detect, interpret, and fix source-file warnings in iterative workflows.
- Provide a small reusable documentation set for both humans and agents.

## Repository Contents

```text
.
|-- README.md
|-- LICENSE
|-- trivadis_warning_status.md
|-- docs
|   |-- ai_guidance.md
|   |-- git_trivadis_workflow.md
|   |-- sqlcl_mcp_file_execution.md
|   \-- manual_trivadis_checks.md
|-- examples
|   |-- select_from_dual_demo.sql
|   \-- select_from_dual_warning_record.md
\-- skills
    \-- sqlcl_mcp_trivadis_agent_skill.md
```

| Path | Description |
| --- | --- |
| `README.md` | Main overview, flow, prompts, and usage notes. |
| `LICENSE` | MIT license for broad reuse with warranty disclaimer. |
| `trivadis_warning_status.md` | One-row-per-file-and-rule warning tracking table with branch and main-merge tracking. |
| `docs/ai_guidance.md` | MCP workflow guidance for source-file warning handling. |
| `docs/git_trivadis_workflow.md` | Branch, commit, PR, review, and merge workflow for Trivadis iterations. |
| `docs/sqlcl_mcp_file_execution.md` | SQLcl MCP `@` and `@@` execution notes, including permission caveats. |
| `docs/manual_trivadis_checks.md` | Standalone SQLcl steps for manual Trivadis checks. |
| `examples/select_from_dual_demo.sql` | Sample script with version comments and fixed warning. |
| `examples/select_from_dual_warning_record.md` | Warning capture and post-fix verification notes. |
| `skills/sqlcl_mcp_trivadis_agent_skill.md` | Skill behavior for SQLcl MCP plus Trivadis iterations. |

## How It Works

1. An agent or human prepares a SQL or PL/SQL script.
2. SQLcl MCP connects to Oracle using a saved SQLcl connection.
3. SQLcl session enables `set codescan on`.
4. The script runs with `@file.sql`.
5. SQLcl emits Trivadis warnings inline, if any exist.
6. The agent updates repository source files for low-risk fixes or asks the user to decide.
7. The script is re-run until warnings are fixed in source, accepted, or recorded.

## Sequence Diagram

```text
User
  |
  | ask for SQL change or review
  v
AI Agent
  |
  | connect + get schema context
  v
SQLcl MCP
  |
  | open saved connection
  v
SQLcl Session --------------------> Oracle Database
  |                                     |
  | set codescan on                     | connect and execute SQL/PLSQL
  | run @script.sql                     |
  v                                     |
Codescan <------------------------------+
  |
  | return Trivadis warnings
  v
SQLcl Session
  |
  | return warnings + query output
  v
SQLcl MCP
  |
  | return execution response
  v
AI Agent
  |
  | update source files or ask user
  v
User
```

## Example Prompts

| Use Case | Prompt |
| --- | --- |
| Direct agent task | Use SQLcl MCP with Trivadis checks for this SQL change. Enable `set codescan on`, run `@file.sql`, update only repository source files for safe fixes, record the warning lifecycle in `trivadis_warning_status.md`, and ask before higher-risk changes. |
| Review existing SQL file | Review this SQL file with SQLcl MCP and Trivadis checks. Run the file, list warning rule numbers, propose the smallest source-file fix for each warning, and apply only safe repository-file edits. |
| Guided step-by-step run | Use the proof-of-concept workflow from this repository. Show each warning, propose the smallest source-file fix, do not modify database objects directly, and keep `trivadis_warning_status.md` updated. |

## Standalone SQLcl Setup

If you want the manual flow to work outside MCP, make sure your local SQLcl supports codescan and enable it in the session before executing scripts.

Basic standalone flow:

```text
sql /nolog
connect my_saved_connection
set codescan on
@/absolute/path/to/your_script.sql
```

Optional folder-level scan:

```text
codescan -path /absolute/path/to/sql/files
```

More detail is in `docs/manual_trivadis_checks.md`.

## Key Rule

Inline Trivadis warnings are not emitted automatically just because a script is executed with `@file.sql`. The SQLcl session must enable `set codescan on`, or the operator must run an explicit `codescan -path ...` command.

## Related Files

- Start with `docs/manual_trivadis_checks.md` for human execution.
- Start with `docs/ai_guidance.md` and `skills/sqlcl_mcp_trivadis_agent_skill.md` for agent-driven workflows.
- Start with `docs/sqlcl_mcp_file_execution.md` for SQLcl MCP file execution details.
- Start with `docs/git_trivadis_workflow.md` for branch, PR, review, and merge workflow.
