# Systems: eBPF Kernel Observability and XDP Network Architecture

## eBPF Virtual Machine Architecture
Extended Berkeley Packet Filter (eBPF) executes sandboxed bytecode inside the Linux kernel without modifying kernel source or loading modules:
1. **Safety Verification**: The in-kernel verifier enforces execution guarantees before loading:
   - Disallows unbounded loops (guarantees termination).
   - Validates memory pointer bounds (prevents null dereference or out-of-bounds reads).
   - Restricts accessible kernel helper functions to program type context.
2. **JIT Compilation**: Converts verified eBPF instructions into native machine code (x86-64, ARM64) for bare-metal execution speeds.

## eBPF Map Primitives
Communication between kernel probes and user-space daemons occurs via shared kernel maps:
- `BPF_MAP_TYPE_HASH`: General key-value store with concurrent lookups.
- `BPF_MAP_TYPE_PERCPU_ARRAY`: Per-core cache-isolated arrays preventing cross-core lock contention.
- `BPF_MAP_TYPE_RINGBUF`: Lockless ring buffer replacing legacy perf buffers for high-volume telemetry dispatch.

## eXpress Data Path (XDP)
XDP executes eBPF programs at the lowest network layer: inside the network device driver before socket buffer (`sk_buff`) allocation:
- Actions: `XDP_DROP` (DDoS line-rate mitigation), `XDP_TX` (packet reflection), `XDP_REDIRECT` (bypassing TCP/IP stack into AF_XDP user buffers).
- Achieves line-rate packet processing exceeding 20 million packets/sec per CPU core.
