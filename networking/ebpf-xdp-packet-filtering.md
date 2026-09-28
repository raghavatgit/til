# eXpress Data Path (XDP): High-Speed Packet Processing

## Architecture
XDP provides high-performance, programmable packet processing directly in the Linux network driver before the kernel allocates a `sk_buff` (socket buffer) data structure.

## Execution Points
1. **Offloaded**: Executed directly on SmartNIC silicon.
2. **Driver (Native)**: Executed in the network interface driver poll routine (NAPI) before `sk_buff` allocation.
3. **Generic**: Fallback execution after `sk_buff` creation (for testing).

## XDP Actions
- `XDP_DROP`: Discard packet immediately with near zero CPU cost (capable of dropping 24+ million packets per second per core).
- `XDP_TX`: Bounce packet back out the same interface (ideal for load balancers).
- `XDP_REDIRECT`: Forward packet to another interface, CPU, or AF_XDP socket.
- `XDP_PASS`: Pass packet up to standard Linux networking stack.
