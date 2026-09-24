# Database Durability: ARIES and Write-Ahead Logging

## Core Invariant: WAL Protocol
Before any dirty page is written from buffer pool memory to disk (Flush), the log record describing the update must already be securely written to stable storage:
$$\text{PageLSN} \le \text{FlushedLSN}$$

## Buffer Management Policies
- **STEAL**: Buffer manager can write dirty pages of uncommitted transactions to disk (requires Undo logging).
- **NO-FORCE**: Buffer manager does not need to write all dirty pages to disk at commit time (requires Redo logging).

## ARIES Three-Phase Recovery
1. **Analysis Phase**: Reconstruct active transaction table and dirty page table from last checkpoint.
2. **Redo Phase**: 'Repeating history' from oldest unwritten log record to restore crash state.
3. **Undo Phase**: Roll back all transactions that were active at crash time, writing Compensation Log Records (CLRs) to ensure idempotency.
