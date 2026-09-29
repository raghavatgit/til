# Read-Copy-Update (RCU) Synchronization

## Core Philosophy
RCU guarantees wait-free, zero-locking reads at the expense of deferred, copying writes.

## The Three Phases
1. Publish: The writer allocates a new copy of the object, modifies it, and updates the global pointer using release memory semantics.
2. Grace Period: The writer waits until all existing reader threads exit their active RCU read-side critical sections (`rcu_read_lock()` to `rcu_read_unlock()`).
3. Reclaim: Once all pre-existing readers complete, the old object memory is safely destroyed (`synchronize_rcu()` or `call_rcu()`).
