# io_uring Registered Fixed Buffers

## Motivation
In standard `io_uring` requests, the kernel must map and pin user-space memory buffers on each read/write call via `get_user_pages()`, incurring page table lookup and reference count increments.

## Registered Buffers Mechanism
Using `io_uring_register(ring_fd, IORING_REGISTER_BUFFERS, iovecs, nr_iovecs)`:
1. The kernel locks the specified memory ranges into RAM (pinning pages).
2. Maps them directly into the kernel's virtual address space once.
3. Subsequent I/O submissions reference buffer indices (`sqe->buf_index`) rather than raw pointers, completely eliminating buffer pinning overhead on high-IOPS NVMe workloads.
