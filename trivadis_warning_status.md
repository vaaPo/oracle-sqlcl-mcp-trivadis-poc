# Trivadis Warning Status

This table tracks one row per source file and Trivadis rule in this proof-of-concept repository.

## Legend

| Col | Meaning |
| --- | --- |
| `SF` | Source file |
| `R` | Trivadis rule code followed by a short note |
| `BR` | Branch name |
| `FDV` | First detected version |
| `LDV` | Last detected version |
| `FIV` | Fixed in version |
| `FS` | Fixed stamp |
| `MM` | Merged to `main` stamp |
| `FN` | Fix notes |

| SF | R | BR | FDV | LDV | FIV | FS | MM | FN |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `examples/select_from_dual_demo.sql` | `G-3145 Avoid using SELECT * directly from a table or view.` | `main` | `v1` | `v1` | `v2` | `2026-06-14T10:51:08+0300` | `2026-06-14T11:47:00+0300` | `Replaced select * from dual with select dummy from dual; verified clean through MCP with set codescan on.` |

## Usage Rule

- Keep one row per source file and Trivadis rule.
- Fill `BR` for the working branch where the warning is tracked.
- Update `Last Detected Version` whenever the same warning is seen again before it is fixed.
- Fill `FIV` and `FS` once the warning is resolved.
- Fill `MM` when the branch is merged to `main`.
- Keep source-file version comments aligned with the latest fixed row.
