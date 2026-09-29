# Log-Structured Merge-Tree (LSM) Architecture and Compaction

## Problem Statement
B-Trees perform in-place updates, causing random I/O write patterns that degrade solid-state drive (SSD) endurance and throughput. LSM-Trees convert all modifications into sequential writes, at the cost of background compaction overhead.

## Layered Hierarchy
1. MemTable: In-memory sorted skiplist receiving real-time writes.
2. WAL (Write-Ahead Log): Sequential append-only disk log guaranteeing crash durability.
3. Immutable MemTable: Frozen snapshot queued for disk flushing.
4. SSTables (Sorted String Tables): Multi-level disk files indexed with Bloom filters.

## Compaction Strategies
- Size-Tiered Compaction: Flushes files of similar sizes into single larger SSTables. Optimized for high write throughput.
- Levelled Compaction: Divides disk storage into exponential levels (L1, L2, ... Ln). Each level has non-overlapping key ranges, minimizing read amplification.
