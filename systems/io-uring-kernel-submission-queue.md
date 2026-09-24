# Linux io_uring: Ring Buffer Asynchronous I/O

## Ring Buffers Design
`io_uring` uses two lock-free ring buffers shared between userspace and the Linux kernel:
1. **Submission Queue (SQ)**: Userspace writes Submission Queue Entries (`sqe`).
2. **Completion Queue (CQ)**: Kernel writes Completion Queue Entries (`cqe`).

## SQPOLL Mode (True Zero-Syscall I/O)
When initialized with `IORING_SETUP_SQPOLL`, a dedicated kernel thread polls the submission queue continuously.
Applications can submit thousands of asynchronous read/write requests simply by writing to shared memory without invoking `enter` system calls.

## Key Advantages Over epoll + read
- Zero context switches per I/O in SQPOLL mode.
- Supports disk files (which epoll does not support asynchronously on Linux).
- Automatic buffer registration via `IORING_REGISTER_BUFFERS` avoids page pinning overhead.
