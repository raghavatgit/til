# B+ Trees vs LSM Trees: Write Amplification and Trade-offs

## B+ Trees (e.g., PostgreSQL, InnoDB)
- Read-optimized: B+ Trees maintain sorted page nodes on disk. Point lookups require $O(\log_B N)$ I/O reads.
- High Write Amplification: Modifying a single 32-byte row requires writing an entire 8 KB or 16 KB page to disk, plus Write-Ahead Log entries.

## LSM Trees (Log-Structured Merge-Tree, e.g., RocksDB, Cassandra)
- Write-optimized: All writes append sequentially to an in-memory `MemTable` (SkipList) and Write-Ahead Log.
- Flushed to immutable Level 0 SSTables (Sorted String Tables).
- Compaction (Size-Tiered or Leveled) merges SSTables in background to maintain read efficiency.
- Trade-off: Lower write latency and higher sequential write throughput at the cost of higher background compaction I/O and slower read amplification.
