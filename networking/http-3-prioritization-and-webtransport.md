# Networking: HTTP/3 Extensible Prioritization and WebTransport

## The Failure of HTTP/2 Priority Trees
HTTP/2 defined a complex dependency tree mechanism with weights and exclusive flags:
- In practice, client implementations constructed differing tree shapes.
- Intermediate proxies rarely translated tree nodes accurately, causing head-of-line prioritization stalls.

## RFC 9218 Extensible Priority Scheme
HTTP/3 replaces complex trees with two orthogonal parameters:
1. **Urgency ($u=0$ to $u=7$)**: 0 is highest urgency (e.g. render-blocking CSS/JS); 7 is lowest (e.g. prefetch).
2. **Incremental ($i$)**: Boolean flag indicating whether streams should be multiplexed concurrently ($i=1$, e.g. progressive images) or serialized ($i=0$).

## WebTransport over HTTP/3
Built directly on top of QUIC:
- Unidirectional and bidirectional streams with independent flow control.
- Datagrams for unreliable, out-of-order, low-latency transmission (ideal for real-time multiplayer telemetry and live video streaming).
