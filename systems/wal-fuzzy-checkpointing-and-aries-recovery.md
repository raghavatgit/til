# Write-Ahead Logging: Fuzzy Checkpointing & ARIES Recovery

## Core Principles of ARIES
Algorithms for Recovery and Isolation Exploiting Semantics (ARIES) is the industry standard crash-recovery algorithm used in relational storage engines (PostgreSQL, SQLite, InnoDB).

---

## The Three Recovery Passes

1. **Analysis Pass**:
   - Scans the log forward from the most recent valid Checkpoint.
   - Reconstructs the Transaction Table (identifying active/uncommitted transactions at crash time).
   - Reconstructs the Dirty Page Table (identifying minimum `RecLSN` requiring Redo).

2. **Redo Pass (Repeating History)**:
   - Scans forward from the lowest `RecLSN` across all dirty pages.
   - Reapplies all logged physical modifications, including changes of uncommitted transactions.
   - Restores database memory buffers to the exact physical state at the millisecond of crash.

3. **Undo Pass**:
   - Scans backward through active transactions from the crash time.
   - Rolls back uncommitted operations in reverse chronological order.
   - Writes **Compensation Log Records (CLRs)** for each undone operation to prevent cascading undos on repeated crashes during recovery.

---

## Fuzzy Checkpointing vs Sharp Checkpointing
- **Sharp Checkpoint**: Flushes all dirty buffers to disk synchronously, blocking transaction commit pipelines.
- **Fuzzy Checkpoint**: Logs active transactions and dirty page lists (`CheckPointBegin`, `CheckPointEnd`), while background writer threads asynchronously stream dirty pages to disk without stalling concurrent reads and writes.
