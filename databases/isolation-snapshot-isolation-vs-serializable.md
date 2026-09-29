# Snapshot Isolation vs Serializable Snapshot Isolation (SSI)

## Snapshot Isolation (SI)
- Each transaction reads from a snapshot of the database at start time.
- Prevents: Dirty reads, Non-repeatable reads, Phantom reads.
- Vulnerability: **Write Skew** anomaly (concurrent doctors on call resigning simultaneously).

## Serializable Snapshot Isolation (SSI, Cahill et al.)
- Implemented in PostgreSQL (`ISOLATION LEVEL SERIALIZABLE`).
- Tracks read-write dependency conflicts (rw-antidependencies) at runtime using SIREAD locks on tuples and pages.
- Detects cycles of `T1 -rw-> T2 -rw-> T3 -rw-> T1` in the Serialization Graph and aborts the pivot transaction with serialization failure.
