# eBPF XDP: Extreme High-Performance Packet Processing

## Architecture Overview
eXpress Data Path (XDP) provides a bare-metal packet processing pipeline inside the Linux network driver before `sk_buff` allocation.
Packets are inspected directly in DMA ring buffers:
- `XDP_DROP`: Discard packet at NIC driver level (ideal for multi-Gbps DDoS mitigation).
- `XDP_TX`: Bounce packet back out the same interface it arrived on.
- `XDP_REDIRECT`: Forward packet to another NIC interface or AF_XDP userspace socket.
- `XDP_PASS`: Pass packet up to standard Linux network stack.

## Memory Model
```c
SEC("xdp")
int xdp_filter(struct xdp_md *ctx) {
    void *data = (void *)(long)ctx->data;
    void *data_end = (void *)(long)ctx->data_end;
    struct ethhdr *eth = data;

    if ((void *)(eth + 1) > data_end)
        return XDP_PASS;

    if (eth->h_proto == __constant_htons(ETH_P_IP)) {
        // Direct packet parsing without kernel copy
    }
    return XDP_PASS;
}
```
