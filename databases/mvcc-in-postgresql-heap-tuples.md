# PostgreSQL MVCC: Heap Tuple Header Internals

## Multi-Version Concurrency Control (MVCC)
Ensures readers never block writers and writers never block readers by maintaining multiple physical versions of rows.

## Heap Tuple Header Fields
Every table row contains metadata headers:
- `xmin`: 32-bit Transaction ID (XID) of the transaction that inserted this row.
- `xmax`: 32-bit XID of the transaction that deleted or updated this row (0 if active).
- `t_ctid`: Points to the newest version of this tuple if updated.

## Tuple Visibility Rule (Simplified)
A row is visible to a transaction with snapshot snapshot if:
1. `xmin` is committed and was committed BEFORE current transaction snapshot was taken.
2. `xmax` is 0, or is aborted, or was committed AFTER current transaction snapshot was taken.
