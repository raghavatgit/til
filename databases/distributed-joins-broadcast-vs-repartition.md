# Distributed Joins: Broadcast Hash Join vs Repartition Shuffle

Massively Parallel Processing (MPP) query engines select physical join operators based on table size statistics.

## Strategies
- **Broadcast Hash Join (BHJ)**: Small table is duplicated across all worker nodes. Large table remains stationary. Zero network shuffle on large table.
- **Hash Repartition Join (Shuffle)**: Both tables are re-partitioned using `hash(join_key) % num_nodes`. Required when both inputs exceed worker RAM.
