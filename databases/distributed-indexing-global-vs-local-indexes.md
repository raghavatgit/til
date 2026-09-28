# Distributed Secondary Indexing: Local vs Global Indexes

## Local Secondary Index (LSI)
- Secondary index is co-located on the same shard as the partition key.
- **Write Cost**: Low. Updates are atomic within the single shard.
- **Read Cost**: High. Queries filtering by secondary index without partition key must scatter-gather to all shards.

## Global Secondary Index (GSI)
- Secondary index is partitioned independently across the cluster using the indexed attribute as its own partition key.
- **Write Cost**: High. Requires distributed transaction (or asynchronous eventual consistency replication).
- **Read Cost**: Low. Queries route directly to the specific shard holding the secondary index partition.
