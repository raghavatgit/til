# TCP Congestion Control: BBR vs CUBIC

## The Problem with Loss-Based Congestion (CUBIC/Reno)
Traditional TCP congestion control treats packet loss as a signal of network saturation.
On modern networks with deep buffer queues (Bufferbloat), loss-based algorithms fill intermediate router buffers until overflowing, creating hundreds of milliseconds of artificial latency.

## BBR (Bottleneck Bandwidth and Round-trip propagation time)
- Formulates congestion as a dual physical constraint: Bottleneck Bandwidth ($BtlBw$) and Minimum Round Trip Time ($RTprop$).
- Targets the optimal operating point: Kleinrock's congestion cliff where pipe is full without queuing delay.
- Maximizes throughput while minimizing end-to-end packet latency.
