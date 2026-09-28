# io_uring: Architecture and Zero-Syscall Asynchronous I/O

## Introduction
Introduced in Linux 5.1, `io_uring` replaces legacy AIO by providing a lockless, shared-memory interface between user space and kernel space.

## Ring Buffers
1. **Submission Queue (SQ)**: Ring buffer of `io_uring_sqe` entries where user-space deposits I/O requests.
2. **Completion Queue (CQ)**: Ring buffer of `io_uring_cqe` entries where the kernel posts completed event statuses.

## Memory Mapping
Both queues are mapped into user space via `mmap()`. By reading and writing pointers directly, operations can be submitted and reaped without context-switching into kernel space via traditional syscalls.

## SQPOLL Mode (Zero Syscall)
When initialized with `IORING_SETUP_SQPOLL`, a dedicated kernel thread polls the submission queue. User-space simply updates the tail pointer using atomic memory operations, achieving true zero-syscall asynchronous operations.
