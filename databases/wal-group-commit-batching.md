# WAL Group Commit Batching Mechanics

## The `fsync` Bottleneck in ACID Transactions
In relational databases (PostgreSQL, MySQL InnoDB, SQLite), the "Durability" guarantee in ACID requires flushing the Write-Ahead Log (WAL) to non-volatile storage before confirming transaction commit. 

A naive implementation calls `fsync()` or `fdatasync()` per transaction commit. Even high-end NVMe drives are physically limited to 50,000-100,000 IOPs, capping single-threaded sequential syncs to a fraction of available compute bandwidth.

---

## Group Commit Architecture

Group Commit aggregates multiple concurrent transaction commits into a single batched `fsync()` call:

```
Thread 1 (Commit Tx 101) ---Thread 2 (Commit Tx 102) ----+--> [ Group Commit Lock-Free Queue ]
Thread 3 (Commit Tx 103) ---/                  |
                                       Leader Thread
                                              |
                                              v
                                   1. Acquire Log Flush LSN
                                   2. Flush WAL Buffer up to Max(LSN)
                                   3. Single fsync() Syscall
                                   4. Wake up all waiting Followers
```

### Leader-Follower Delegation Flow
1. **Leader Election:** The first thread to initiate commit acquires the flush lock and becomes the Group Leader.
2. **Follower Registration:** Concurrent committing threads append their transaction LSNs to a lock-free queue and sleep on a condition variable or futex.
3. **Batched Sync:** The Leader writes all pending log records up to the highest queued LSN and issues a single `fsync()`.
4. **Group Wakeup:** The Leader releases all waiting followers simultaneously, satisfying durability for dozens of transactions in one physical disk sync.

---

## Benchmark Impact
* **Without Group Commit:** 800 - 1,200 commits/sec (bounded by disk sync latency).
* **With Group Commit:** 25,000 - 80,000 commits/sec under high concurrency.
