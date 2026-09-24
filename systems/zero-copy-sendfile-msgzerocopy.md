# Linux Zero-Copy Networking: sendfile vs splice vs MSG_ZEROCOPY

## 1. `sendfile(out_fd, in_fd, offset, count)`
Transfers data directly between a file descriptor and a socket without copying data into user space buffers. Kernel transfers page cache references directly to socket buffers.

## 2. `splice(fd_in, off_in, fd_out, off_out, len, flags)`
Moves data between two arbitrary file descriptors via a Linux pipe buffer without userspace copying.

## 3. `MSG_ZEROCOPY` (Socket send flag)
Available on Linux 4.14+:
Locks userspace memory pages and passes physical page pointers directly to network device DMA. Kernel notifies userspace via socket error queue when DMA completes and buffers can be safely reused.
