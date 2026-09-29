# eBPF XDP_REDIRECT and AF_XDP Zero-Copy

## XDP Action Codes
- `XDP_DROP`: Discards packet at line rate (DDoS protection).
- `XDP_TX`: Bounces packet back out the receiving network interface.
- `XDP_REDIRECT`: Forwards packet directly to another NIC, CPU core, or userspace AF_XDP socket.

## AF_XDP Architecture
Userspace memory pools (UMEM) are mapped directly into NIC ring buffers via `mmap()`, enabling 100GbE packet capture without copying bytes through the Linux kernel network stack.
