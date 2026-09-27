# Linux futex2 & sys_futex_waitv

## Motivation: Multi-Futex Wait
Classic `futex(2)` only allowed a thread to sleep on a single 32-bit integer memory address. Windows had `WaitForMultipleObjects()`, but Linux threads waiting on multiple synchronization primitives (e.g. Vulkan / Wine / Proton gaming engines) had to wake up via inefficient eventfd or socketpair polling.

---

## futex_waitv Interface
Introduced in Linux 5.16, `sys_futex_waitv()` allows a thread to wait on an array of `struct futex_waitv` items:

```c
struct futex_waitv {
    __u64 val;
    __u64 uaddr;
    __u32 flags;
    __u32 __reserved;
};
```

Supports atomic wakeup when *any* of the watched futexes change, dramatically cutting CPU context switch overhead in multithreaded runtimes.
