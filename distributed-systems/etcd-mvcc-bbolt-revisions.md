# etcd MVCC Multi-Version Revision Store Architecture

etcd persists distributed state through a 64-bit monotonically increasing `revision` counter backed by an in-memory B-tree index and an on-disk `bbolt` key-value engine.

## Revision Composition
- `main`: Revision index incremented on every transaction.
- `sub`: Sub-revision index tracking multi-key mutations within the same transaction.

## Compaction Mechanics
Periodic or retention-based compaction purges key revisions older than target revision `R`, preventing unbounded database growth and reclaiming disk blocks via bbolt freelist.
