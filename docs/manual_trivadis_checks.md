# Manual Trivadis Checks In SQLcl

## Purpose

This guide shows how a human operator can check a SQL file with SQLcl and see Trivadis warnings directly in the terminal.

## Preconditions

- SQLcl is installed.
- A saved SQLcl connection already exists.
- The target SQL file is available on the local filesystem.

## Inline Check Flow

1. Start SQLcl.
2. Connect using a saved connection.
3. Enable Trivadis checks in the current session.
4. Run the SQL file with `@`.
5. Inspect the warning output.

Example session:

```text
sql /nolog
connect my_saved_connection
set codescan on
@/absolute/path/to/your_script.sql
```

Example warning:

```text
SQL best practice warning (1,8): G-3145: Avoid using SELECT * directly from a table or view
```

## Folder-Level Scan

Use `codescan` when you want to scan many files at once:

```text
codescan -path /absolute/path/to/sql/files
```

Structured JSON output:

```text
codescan -path /absolute/path/to/sql/files -format json -output /absolute/path/to/report.json
```

## Human Review Checklist

1. Confirm the warning rule number, for example `G-3145`.
2. Identify the file and line number.
3. Decide whether the fix is mechanical or needs design judgment.
4. Apply the smallest correct fix.
5. Re-run the file with `set codescan on` still enabled.
6. Record the outcome in `trivadis_warning_status.md`.

## Common Pitfall

If you run `@file.sql` without `set codescan on`, SQLcl may execute the script successfully without showing any Trivadis warning.
