# Multi-Raft in Distributed SQL: CockroachDB and TiDB

## The Monolithic Raft Bottleneck
A single Raft consensus group cannot scale beyond the bandwidth of a single leader node.

## Multi-Raft Architecture
1. Shard data into Ranges (e.g., 64 MB blocks).
2. Each Range is an independent, autonomous Raft consensus group with its own leader and followers.
3. A single physical server hosts hundreds of range replicas, participating as Leader for some ranges and Follower for others.
4. Splits: When a range grows beyond threshold, it splits into two ranges and updates the cluster routing range descriptor table.
