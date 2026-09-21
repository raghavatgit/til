# Linux Kernel: Zero-Copy Data Movement via splice(2) and vmsplice(2)

## The Mechanism of Pipe Buffers (struct pipe_inode_info)
Linux pipes are represented in the kernel as circular arrays of `struct pipe_buffer`:
- Each buffer points to a physical `struct page` in RAM with an offset and length.
- Data does not need to be copied into a contiguous memory chunk.

## splice(2) System Call
Moves data between a file descriptor and a pipe without copying to user space:
```c
#define _GNU_SOURCE
#include <fcntl.h>
ssize_t splice(int fd_in, loff_t *off_in, int fd_out, loff_t *off_out, size_t len, unsigned int flags);
```
- Disk file -> Pipe -> Network Socket:
  1. `splice(file_fd, ..., pipe_write_fd, ...)` fills pipe buffers with page references from the kernel Page Cache.
  2. `splice(pipe_read_fd, ..., socket_fd, ...)` passes page pointers directly to network device drivers.
  3. CPU does zero memory copying.

## vmsplice(2) Zero-Copy from User Memory
Maps user-space virtual memory directly into a kernel pipe:
- Flags: `SPLICE_F_GIFT`: User space gifts pages to the kernel without copy (pages must be page-aligned).
