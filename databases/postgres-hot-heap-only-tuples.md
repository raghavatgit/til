# PostgreSQL Heap-Only Tuples (HOT)

## The Write Amplification Problem in MVCC
When an `UPDATE` executes in Postgres, MVCC creates a new tuple version. Naively, every index referencing the table must insert an entry pointing to the new physical tuple pointer (ctid), causing massive index bloat.

---

## HOT Optimization Conditions
If an `UPDATE` satisfies two conditions:
1. No indexed column values are modified.
2. The new tuple version fits inside the same 8 KB heap page as the old tuple.

Postgres chains the old tuple to the new tuple via an in-page pointer. Indexes continue pointing to the root tuple, completely eliminating index write amplification.
