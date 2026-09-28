# Horizontal Database Sharding: Hash vs Range Partitioning

## Range Partitioning
- Divides data based on contiguous ranges of a column value (e.g., timestamps or alphabetical ranges).
- Advantage: Range scans (`SELECT WHERE timestamp BETWEEN A AND B`) route to a single shard.
- Disadvantage: Severe write hotspotting when appending time-series data (all writes hit the newest shard).

## Hash Partitioning
- Applies a hash function to the partition key (`hash(user_id) mod N`).
- Advantage: Uniform distribution of write traffic across all cluster shards.
- Disadvantage: Range queries must scatter-gather across all shards and merge in memory.
