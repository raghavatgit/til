# Linux Futex (Fast Userspace Mutex) Internals

## The Cost of Traditional System Call Synchronization
Prior to `futex`, acquiring or releasing a mutex required a trap into kernel mode (`pthread_mutex` calling `sys_semop`), costing ~100-200 nanoseconds even in the uncontended case.

---

## Futex Design Principle: Uncontended Fast-Path in Userspace
A `futex` consists of an integer in userspace memory aligned to 4 bytes:
1. **Uncontended Lock**:
   - The thread executes an atomic Compare-And-Swap (`lock cmpxchg`).
   - If the integer transitions from `0` (unlocked) to `1` (locked), the lock is acquired immediately without invoking a system call.

2. **Contended Lock**:
   - If the CAS fails, the thread sets the state to `2` (locked with waiting threads) and invokes `sys_futex(val_addr, FUTEX_WAIT, 2, NULL)`.
   - The kernel validates that `*val_addr == 2` under an internal hash-bucket lock:
     - If true: thread is placed on the kernel wait-queue and suspended (`TASK_INTERRUPTIBLE`).
     - If false: system call returns immediately (`EWOULDBLOCK`) to re-attempt userspace CAS.

3. **Unlock**:
   - Decrement integer. If it was `1` (no waiters), return immediately.
   - If it was `2`, invoke `sys_futex(val_addr, FUTEX_WAKE, 1)` to awaken one sleeping waiter from the kernel queue.
