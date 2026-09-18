# Systems: Sequence Locks (SeqLocks) and Read-Copy-Update (RCU)

## The Problem with Reader-Writer Spinlocks
Standard `rwlock_t` primitives suffer from writer starvation or cache line invalidation during reader acquisition:
- Even when reading, readers must increment an atomic counter in shared cache lines.
- On multi-socket NUMA systems, this triggers high bus interconnect traffic.

## Sequence Locks (SeqLock Architecture)
In a SeqLock (`seqlock_t`):
- A single 32-bit/64-bit integer sequence counter is maintained.
- Sequence is even when no writer is active.
- A writer increments the sequence to an odd number before writing, updates state, and increments to an even number after writing.

### Reader Protocol (Lock-Free)
1. Read initial sequence counter $S_1$. If odd, spin until even.
2. Read protected data fields.
3. Read final sequence counter $S_2$.
4. If $S_1 \neq S_2$, a writer modified the state concurrently. Retry from step 1.
- **Key Advantage**: Readers perform zero writes to shared cache lines. High concurrency without reader-reader contention.

## Read-Copy-Update (RCU)
RCU is optimized for read-mostly linked data structures:
- Readers execute inside an RCU read-side critical section (`rcu_read_lock()` / `rcu_read_unlock()`) with zero hardware atomic operations.
- Writers allocate a new copy of a node, update fields, and atomically replace the pointer.
- The old node memory is deferred until all existing readers finish their grace period (`synchronize_rcu()` or `call_rcu()`).
