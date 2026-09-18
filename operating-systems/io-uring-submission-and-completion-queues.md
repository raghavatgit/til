# Linux Kernel: io_uring Architecture and Kernel Polling (SQPOLL)

## The Fundamental Overhead of System Calls
Traditional Linux asynchronous I/O (`aio`) or synchronous calls (`pread`, `pwrite`):
- Require at least one `syscall` per batch of I/O operations.
- Incurs hardware user-kernel context transitions, register saving/restoration, and Spectre/Meltdown mitigation page table swaps (KPTI).

## Dual Ring Buffer Architecture
`io_uring` establishes two lockless ring buffers shared between user space and the kernel via a single `mmap(2)`:
1. **Submission Queue (SQ)**: Ring buffer of `io_uring_sqe` descriptors populated by user space.
2. **Completion Queue (CQ)**: Ring buffer of `io_uring_cqe` result events populated by the kernel.

Pointers are updated with atomic memory store-release and load-acquire semantics without taking locks.

## True Zero-Syscall I/O with IORING_SETUP_SQPOLL
When initialized with the `IORING_SETUP_SQPOLL` flag:
- Kernel spawns a dedicated kernel worker thread (`io_uring-sq`).
- The kernel thread continuously polls the Submission Queue for incoming requests.
- User space writes requests to the SQ ring and updates the tail pointer without issuing any `enter(2)` system call.
- Delivers millions of IOPS with zero context-switch overhead.
