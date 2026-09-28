# TCP BBR vs CUBIC Congestion Control

## Loss-Based Congestion Control (CUBIC)
- Assumes packet loss equals network congestion.
- Continues accelerating sending rate until router buffers overflow (Bufferbloat), inducing massive packet latency before backoff.

## Model-Based Congestion Control (BBR)
- Developed by Google, Bottleneck Bandwidth and Round-trip propagation time (BBR) models the physical pipe.
- Operates at the Kleinrock optimum: sending rate equals bottleneck bandwidth ($BtlBw$), and in-flight data equals bandwidth-delay product ($BDP = BtlBw \times RTprop$).
- Results: Substantially higher throughput on lossy links (Wi-Fi, satellite) and minimal queuing delay.
