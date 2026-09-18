# Networking: BBR Congestion Control vs Loss-Based TCP Cubic

## The Bufferbloat Problem of Loss-Based Congestion Control
Traditional TCP congestion algorithms (Reno, Cubic) treat packet loss as the primary signal of network congestion:
- They aggressively increase congestion window ($cwnd$) until bottleneck router buffers overflow and drop packets.
- Result: Deep intermediate network buffers fill up, introducing hundreds of milliseconds of artificial round-trip latency without increasing throughput (**Bufferbloat**).

## Bottleneck Bandwidth and Round-trip Time (BBR)
Developed by Google (Cardwell et al.), BBR decouples packet loss from congestion:
- A network path has two fundamental physical limits:
  1. **Bottleneck Bandwidth ($BtlBw$)**: The maximum throughput the slowest link can support.
  2. **Round-Trip Propagation Time ($RTprop$)**: The minimum physical flight time of packets.
- The optimal operating point (Kleinrock's optimal operating point) occurs when the inflight data equals the Bandwidth-Delay Product:
  $\text{BDP} = BtlBw \times RTprop$

## Operating State Machine
BBR alternates through 4 states:
1. **Startup**: Exponential bandwidth probing.
2. **Drain**: Clears any residual queue built during startup.
3. **ProbeBW**: Periodically pulses pacing rate by $+25\%$ to discover new bandwidth, followed by $-25\%$ to drain queues.
4. **ProbeRTT**: Drops inflight packets to 4 for 200ms every 10 seconds to accurately measure true physical $RTprop$.
