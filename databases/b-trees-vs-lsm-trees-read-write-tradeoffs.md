# Databases: B+ Trees vs LSM Trees Read/Write Trade-Offs

## In-Place Update (B+ Tree)
Engines: PostgreSQL, MySQL InnoDB, SQLite, MongoDB WiredTiger.
- **Data Organization**: Balanced N-ary tree with pages (typically 4KB - 16KB). Keys and pointers in interior nodes; data records in leaf nodes.
- **Write Path**: Modifying a record updates the page in-place. Dirty pages are logged to WAL and flushed via background checkpoints.
- **Trade-off**:
  - High Read Performance: $O(\log_B N)$ page reads with minimal amplification.
  - High Write Amplification: Modifying 10 bytes can rewrite a full 16KB page ($WAF \approx 1000\times$).
  - Random write I/O bounds throughput on magnetic disks and SSD flash blocks.

## Append-Only Log Structured Merge (LSM Tree)
Engines: RocksDB, Apache Cassandra, ScyllaDB, CockroachDB.
- **Data Organization**: MemTable (RAM skip list) + SSTables (Sorted String Tables) in hierarchical levels on disk.
- **Write Path**: Writes append to in-memory MemTable and disk WAL. No in-place disk page updates.
- **Trade-off**:
  - Unrivaled Write Throughput: Converts random writes into high-speed sequential disk I/O.
  - Higher Read Amplification: May check multiple SSTables (mitigated by Bloom filters).
  - Background compaction consumes background disk bandwidth.
