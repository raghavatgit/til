# Linux futex (Fast Userspace Mutex) Internals

## Mechanism
A futex consists of a 32-bit integer in user-space memory and a kernel-level wait queue. 
- **Uncontended Case**: The locking thread performs an atomic Compare-And-Swap (CAS) on the user-space integer. If successful, acquisition completes in user space with zero kernel overhead.
- **Contended Case**: If CAS fails, the thread invokes `futex(val_addr, FUTEX_WAIT, expected_val, timeout)`. The kernel verifies that `*val_addr == expected_val` atomically with descheduling the calling thread and enqueueing its `task_struct` onto a hash bucket wait queue.

## Wakeup
Releasing threads execute CAS to unlock. If contention flags indicate waiting threads, the locker invokes `futex(val_addr, FUTEX_WAKE, count)` to transition waiters back to `TASK_RUNNING`.
