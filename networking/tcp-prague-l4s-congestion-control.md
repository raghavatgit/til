# TCP Prague and L4S (Low Latency, Low Loss, Scalable)

## The Bufferbloat Conflict
Traditional TCP Reno and CUBIC require dropping packets to discover link capacity, forcing network routers to maintain large queues that induce latency spikes.

## L4S Architecture (RFC 9330)
- **DualQ Coupled AQM**: Routers maintain two queues: Classic (C) and L4S (L).
- **Prague Congestion Control**: Uses Explicit Congestion Notification (ECN) markings (`CE` codepoint) per round-trip time.
- Adjusts window smoothly: Decreases sending window proportional to the fraction of marked packets rather than halving, sustaining ultra-low queuing delay (<1ms) alongside maximum throughput.
