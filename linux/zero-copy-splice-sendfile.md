# Zero-Copy I/O: sendfile and splice

## Traditional Read/Write Overhead
1. `read()`: DMA copies data from NIC/Disk to kernel page cache. CPU copies data from kernel page cache to user-space buffer (Context switch 1).
2. `write()`: CPU copies data from user space back to socket buffer in kernel space (Context switch 2). DMA copies socket buffer to NIC.
Total: 4 context switches, 2 CPU memory copies, 2 DMA transfers.

## sendfile()
Transfers data directly between file descriptors in kernel space:
`sendfile(out_fd, in_fd, &offset, count)`
Eliminates user-space copying. When NIC supports scatter-gather DMA, CPU copying is eliminated entirely: DMA reads directly from page cache.

## splice()
Generalizes zero-copy to arbitrary pipes by manipulating `pipe_buffer` page pointer references without copying raw bytes.
