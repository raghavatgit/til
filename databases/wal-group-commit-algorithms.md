# Write-Ahead Log Group Commit and fsync Pipeline Batching

Storage engines batch concurrent transaction commits into single synchronous `fsync()` system calls to prevent disk rotational/NVMe queue bottlenecks.

## Algorithm
1. First committing transaction becomes the **Group Leader**.
2. Concurrent arriving transactions register on a lock-free queue as **Followers**.
3. Group Leader flushes WAL buffer up to latest sequence number and executes single `fsync()`.
4. Group Leader notifies all followers, committing N transactions in 1 physical disk sync.
