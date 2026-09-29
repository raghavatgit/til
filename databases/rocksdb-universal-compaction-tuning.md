# RocksDB Universal Compaction Strategy

Universal compaction organizes SST files in chronological order rather than strict geometric levels, optimizing write amplification for append-heavy workloads.

## Trigger Conditions
1. **Number of files**: Compaction triggers when total sorted runs exceed `level0_file_num_compaction_trigger`.
2. **Size amplification**: Compaction triggers when ratio of total database size to oldest run exceeds `max_size_amplification_percent`.
3. **Read amplification**: Lower sorted runs reduce point lookup latency.
