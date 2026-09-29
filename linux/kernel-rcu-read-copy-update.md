# Read-Copy Update (RCU) in the Linux Kernel

## Core Mechanics
RCU enables concurrent, lock-free reads while allowing mutation:
1. **Publish**: Writer creates a modified copy of an existing data structure and updates the global pointer atomically using memory release barriers (`rcu_assign_pointer`).
2. **Subscribe**: Readers dereference the pointer within an RCU read-side critical section (`rcu_read_lock()` / `rcu_read_unlock()`).
3. **Grace Period**: Memory for the obsolete data version is reclaimed only after all existing readers have exited their critical sections (`synchronize_rcu()` or `call_rcu()`).
