# High-Throughput I/O Architecture: io_uring vs Epoll

## Architectural Differences
- `epoll` is a readiness notification mechanism: It informs userspace that a file descriptor is ready for non-blocking read/write, requiring separate subsequent syscalls (`read()`, `write()`).
- `io_uring` is a true asynchronous completion queue mechanism: Userspace submits I/O requests into a shared-memory Submission Queue (SQ), and the kernel deposits results into a Completion Queue (CQ) without syscall switches.

## Ring Buffer Architecture
Shared memory rings eliminate context switch overhead:
1. Submission Queue (SQ): Userspace writes submission queue entries (SQEs) and updates the SQ tail.
2. Completion Queue (CQ): Kernel processes requests and writes completion queue entries (CQEs), updating the CQ tail.
3. Polling Mode (`IORING_SETUP_SQPOLL`): A dedicated kernel thread continuously polls the SQ, achieving zero-syscall I/O.
