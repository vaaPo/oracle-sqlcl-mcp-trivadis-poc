# SQLcl MCP File Execution Notes

## Goal

Explain how repository SQL files should be executed through SQLcl MCP, including common permission and configuration considerations.

## Use SQLcl Commands, Not Shell Wrappers

- Prefer `sqlcl_sqlcl_run` for script execution through MCP.
- Use `@/absolute/path/to/script.sql` for a direct script.
- Use `@@nested_script.sql` only when the nested path should resolve relative to the calling script.
- Avoid routing file execution through a shell command when the goal is SQLcl behavior and inline Trivadis warnings.

## Important Behavior

- `@` and `@@` are SQLcl commands, not plain SQL statements.
- Because of that, they must be executed with SQLcl command execution support.
- `set codescan on` must be enabled in the same SQLcl session before running the file if inline Trivadis warnings are expected.

## MCP Configuration Caveat

- SQLcl file execution through MCP may require explicit tool or permission configuration in the MCP host environment.
- In some setups this is not enabled by default.
- The exact mechanism depends on the MCP client, permission model, and local policy configuration.
- If `@file.sql` or `@@file.sql` works in interactive SQLcl but not through MCP, check MCP command permissions first.

## Recommended MCP Pattern

```text
sqlcl_connect
sqlcl_schema_information
sqlcl_sqlcl_run: set codescan on
sqlcl_sqlcl_run: @/absolute/path/to/your_script.sql
```

Nested script example:

```text
sqlcl_sqlcl_run: @@nested_script.sql
```

## Practical Guidance

- Use absolute paths for top-level script execution.
- Keep repository source files under version control and validate those files through MCP.
- Prefer editing source files first, then executing them via SQLcl MCP for validation.
