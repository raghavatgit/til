# Networking: TCP TIME_WAIT State and SO_REUSEADDR

## Purpose of TIME_WAIT
The active closer of a TCP connection enters `TIME_WAIT` for $2 \times \text{MSL}$ (Maximum Segment Lifetime, default 60 seconds on Linux):
1. **Graceful ACK Delivery**: Ensures the final `ACK` is reliably received by the peer. If the final `ACK` is lost, the peer retransmits `FIN`, which can only be answered if the connection record still exists.
2. **Old Duplicate Segment Drainage**: Prevents delayed phantom packets from a previous connection instance from corrupting a newly established incarnation sharing the same 4-tuple `(src_ip, src_port, dst_ip, dst_port)`.

## High-Throughput Socket Rebinding
On busy reverse proxies, rapid connection turnover exhausts local ephemeral ports ($65535$).
- `SO_REUSEADDR`: Allows immediate binding to a local port in `TIME_WAIT`.
- `tcp_tw_reuse`: Kernel reclaims safe outgoing client sockets in `TIME_WAIT` when timestamps (`RFC 1323`) are monotonic.
