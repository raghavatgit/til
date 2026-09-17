# Log-Structured Merge Trees: Compaction and Write Amplification

## Core LSM Structure
LSM Trees (RocksDB, LevelDB, Cassandra) optimize for write-heavy workloads by converting random disk I/O into sequential append writes:
1. **MemTable**: In-memory skip list or balanced tree buffering recent writes.
2. **Write-Ahead Log (WAL)**: Append-only disk log guaranteeing durability against crashes.
3. **SSTables (Sorted String Tables)**: Immutable, sorted disk files organized in hierarchical levels ($L_0, L_1, \dots, L_k$).

## Compaction Strategies

### 1. Leveled Compaction (RocksDB Default)
- Level 0 holds flushed MemTables (keys may overlap across files).
- Levels 1 through $k$ have non-overlapping key ranges. Each level is roughly $10\times$ the capacity of the previous level.
- When $L_i$ exceeds its byte quota, SSTables are merged into $L_{i+1}$.
- **Trade-off**: Low read amplification ($O(1)$ SSTable per level), high write amplification ($WAF \approx 10 - 30$).

### 2. Size-Tiered Compaction (Cassandra Default)
- Compaction triggers only when multiple SSTables of similar size accumulate.
- SSTables within the same tier have overlapping key ranges.
- **Trade-off**: Lower write amplification ($WAF \approx 4 - 8$), higher read amplification (must check multiple SSTables per read).

## Mathematical Formulation of Write Amplification Factor (WAF)
$$\text{WAF} = \frac{\text{Total Bytes Written to Storage}}{\text{Bytes Written by Application Request}}$$

Optimizing WAF requires tuning Bloom filter bit allocations (e.g. 10 bits/key reduces non-existent key lookups to 1% false positive rate) and adjusting compaction concurrency.
