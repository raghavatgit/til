# Linux futex2: Multiple Wait System Calls

## Limitations of Classic futex
Classic `futex()` only supports waiting on a single memory address at a time. Emulating Windows `WaitForMultipleObjects` required spawning auxiliary monitor threads or polling loops.

## futex2 (`futex_waitv`)
Introduced in Linux 5.16:
`int futex_waitv(struct futex_waitv *waiters, unsigned int nr_waiters, unsigned int flags, struct timespec *timeout, clockid_t clockid)`
- Monitors an array of independent user-space futex words simultaneously.
- Thread sleeps until at least one target word modifies its state or timeout expires.
- Unlocks high-performance game engine synchronization and POSIX mutex clustering.
