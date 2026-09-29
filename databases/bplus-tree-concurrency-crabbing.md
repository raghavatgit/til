# B+Tree Concurrency: Latch Crabbing and Optimistic Coupling

Concurrent B+tree index traversal minimizes root contention through fine-grained reader-writer latches.

## Crabbing Invariant
- **Search**: Acquire Read latch on child, release Read latch on parent.
- **Insert**: Acquire Write latch on child. If child is safe (has space without split), release all ancestor Write latches.
- **Optimistic**: Read latch-free with version counters; validate node version before pointer dereference.
