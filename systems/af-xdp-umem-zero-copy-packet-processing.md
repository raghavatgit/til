# AF_XDP & UMEM Zero-Copy Packet Processing

## The Kernel Network Stack Bottleneck
Linux kernel `sk_buff` (skb) allocation, checksum calculation, netfilter tables, and socket buffer copies introduce significant latency (~1-2 microseconds per packet). At 100 Gbps speeds (148 million packets per second for 64-byte frames), the standard Linux network stack exhausts CPU capacity.

---

## AF_XDP Architecture
AF_XDP (eXpress Data Path Address Family) enables userspace applications to read and write network frames directly from Network Interface Card (NIC) rings without intermediate copies.

### UMEM Memory Area & Ring Buffers
A contiguous memory chunk (`UMEM`) is registered with the kernel driver via `setsockopt(fd, SOL_XDP, XDP_UMEM_REG, ...)`:

1. **Fill Ring (Userspace -> Kernel)**:
   - Userspace pushes empty UMEM buffer addresses (frames) into the Fill Ring.
   - NIC DMA engine writes incoming packets directly into these addresses.

2. **Rx Ring (Kernel -> Userspace)**:
   - Kernel notifies userspace of populated frames containing raw network packets.

3. **Tx Ring (Userspace -> Kernel)**:
   - Userspace enqueues packet descriptors for transmission.

4. **Completion Ring (Kernel -> Userspace)**:
   - Kernel acknowledges transmission completion, allowing userspace to recycle frame descriptors.

---

## Zero-Copy Path Summary
```
NIC DMA -> UMEM Frame -> Userspace Processor (zero copy, zero sk_buff)
Userspace -> Tx Ring -> NIC DMA (zero copy wire transmission)
```
