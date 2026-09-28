# eBPF and eXpress Data Path (XDP) Kernel Packet Filtering

## Execution Pipeline

Traditional Linux packet processing traversals require `netif_receive_skb` allocation, socket buffer (`sk_buff`) construction, iptables/nftables traversal, and socket queue wakeups.

XDP (eXpress Data Path) executes verified eBPF byte code directly inside the network driver before socket buffer allocation occurs:

```
[NIC RX FIFO]
      |
      v
[Driver DMA Ring Buffer]
      |
      v
[eXpress Data Path (XDP) Hook] <--- eBPF Program Execution
      |
      +---> XDP_DROP      (Immediate wire-speed line drop: DDoS filter)
      +---> XDP_TX        (Bounces packet back out the same interface)
      +---> XDP_REDIRECT  (Routes packet to AF_XDP zero-copy socket or another NIC)
      +---> XDP_PASS      (Allocates sk_buff and sends to standard Linux TCP/IP stack)
```

---

## AF_XDP Zero-Copy Socket Architecture

AF_XDP sockets (`XSK`) provide kernel bypass for user-space networking without proprietary drivers (like DPDK):
1. **UMEM Memory Pool**: Pre-allocated contiguous user memory split into fixed-size chunks.
2. **Fill Ring & Rx Ring**: Driver reads frames directly into UMEM chunks.
3. **Tx Ring & Completion Ring**: User space writes zero-copy frames for wire-speed egress.
