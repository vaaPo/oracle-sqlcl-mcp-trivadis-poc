# Trivadis Warning Record

- Connection: `saved SQLcl connection`
- Executed with: `@/absolute/path/to/examples/select_from_dual_demo.sql`
- Requirement discovered: `set codescan on` must be enabled before running `@` scripts if you want SQLcl MCP to surface Trivadis warnings inline.

## Initial Warning

Source version `v1` used:

```sql
select * from dual;
```

Observed output:

```text
SQL best practice warning (1,8): G-3145: Avoid using SELECT * directly from a table or view
"DUMMY"
"X"
```

## Fix Applied

- Fixed in source version `v2`
- Fixed by: `OpenCode / gpt-5.4`
- Fixed at: `2026-06-14T10:51:08+0300`
- Fix: Replaced `select * from dual;` with `select dummy from dual;`

## Verification After Fix

Re-run with `set codescan on` still enabled:

```text
"DUMMY"
"X"
```

- No Trivadis warning was returned after the fix.
