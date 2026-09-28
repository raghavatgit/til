# Linux io_uring Asynchronous I/O Subsystem

## Architectural Principles

Traditional Linux asynchronous I/O (`aio_read`, `aio_write`) suffered from blocking metadata operations, limitation to `O_DIRECT`, and mandatory system call overhead per batch.

`io_uring` provides high-performance, zero-syscall asynchronous operations via two lockless ring buffers mapped into shared user-kernel memory:
1. **Submission Queue (SQ)**: User space pushes Submission Queue Entries (SQE).
2. **Completion Queue (CQ)**: Kernel pushes Completion Queue Events (CQE).

```
User Space                           Kernel Space
+----------------------+             +----------------------+
|  Submission Queue    | --(mmap)--> |  Kernel Worker Ring  |
|  (Head / Tail Ring)  |             |  (SQPOLL Daemon)     |
+----------------------+             +----------------------+
          |                                     |
          v                                     v
+----------------------+             +----------------------+
|  Completion Queue    | <-- (mmap)- |  Async Execution     |
|  (CQE with Result)   |             |  (NVMe / Network)    |
+----------------------+             +----------------------+
```

---

## SQPOLL Zero-Syscall Mode

When configured with `IORING_SETUP_SQPOLL`:
- A kernel thread continuously polls the SQ ring for new entries.
- User space writes SQEs and updates the ring tail with memory barrier semantics (`stdatomic` / compiler barrier).
- No `enter` syscall is invoked during steady-state processing, achieving zero-syscall I/O at millions of IOPS per core.
