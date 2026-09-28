# Write-Ahead Logging (WAL) and ARIES Recovery

## The WAL Invariant
A dirty page in the buffer pool cannot be written to non-volatile disk storage until all log records describing the update are flushed to durable storage:
$$\text{PageLSN} \le \text{FlushedLSN}$$

## ARIES (Algorithms for Recovery and Isolation Exploiting Semantics)
Three-pass recovery after system crash:
1. **Analysis Pass**: Scans WAL forward from last checkpoint to identify dirty pages in buffer pool and active uncommitted transactions at time of crash.
2. **Redo Pass**: Scans forward from smallest unwritten `RecLSN` repeating history, restoring all updates (including aborted transactions).
3. **Undo Pass**: Scans backward undoing changes of all active transactions, writing Compensation Log Records (CLRs) to ensure crash during recovery is idempotent.
