# Seqlock vs RCU: Read-Mostly Concurrency Mechanics

## Seqlock (Sequential Locks)
* Consists of an integer sequence counter and a spinlock.
* Readers read sequence count before and after memory access. If count is odd (write in progress) or changed, readers retry in a loop.
* Ideal for small data structures (like kernel `jiffies` or system time) where writers must never block readers.

---

## Read-Copy-Update (RCU)
* Readers enter `rcu_read_lock()` without modifying shared memory (zero atomic instructions).
* Writers allocate a new copy, update pointer atomically, and defer freeing old memory until all readers pass a "grace period".
* Ideal for large pointer-linked structures (dentries, routing tables).
