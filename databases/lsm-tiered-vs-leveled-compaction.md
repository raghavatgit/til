# Storage: Leveled vs Size-Tiered Compaction in High-Throughput Engines

## The Core Trade-Off
Log-Structured Merge (LSM) trees cannot avoid compaction:
- As SSTables accumulate, point reads must check multiple files, causing read amplification.
- Compaction merges SSTables to remove dead/overwritten keys, but consumes disk write bandwidth (write amplification).

## Leveled Compaction (RocksDB Default)
- $L_0$ contains flushed MemTables (overlapping keys).
- $L_1$ to $L_k$ have strictly non-overlapping key ranges, with each level $10\times$ larger than previous.
- **Metrics**:
  - Read Amplification: Low. A point lookup checks at most 1 SSTable per level.
  - Write Amplification: High ($WAF \approx 10-30$).

## Size-Tiered Compaction (Cassandra Default)
- Groups SSTables into tiers of roughly equal file size.
- Compaction merges $N$ similarly sized tables into a single larger table.
- **Metrics**:
  - Read Amplification: Higher. May check multiple SSTables per tier.
  - Write Amplification: Lower ($WAF \approx 4-8$). Ideal for write-heavy append workloads (time-series metrics).
