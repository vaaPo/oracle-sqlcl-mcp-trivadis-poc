# Trivadis Warning Status

This table tracks one row per source file and Trivadis rule in this proof-of-concept repository.

| Source File | Rule | First Detected Version | Last Detected Version | Trivadis Warn Notes | Fixed In Version | Fixed Stamp | Fix Notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `examples/select_from_dual_demo.sql` | `G-3145` | `v1` | `v1` | `Avoid using SELECT * directly from a table or view.` | `v2` | `2026-06-14T10:51:08+0300` | `Replaced select * from dual with select dummy from dual; verified clean through MCP with set codescan on.` |

## Usage Rule

- Keep one row per source file and Trivadis rule.
- Update `Last Detected Version` whenever the same warning is seen again before it is fixed.
- Fill `Fixed In Version` and `Fixed Stamp` once the warning is resolved.
- Keep source-file version comments aligned with the latest fixed row.
