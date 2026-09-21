# Networking: Nagle's Algorithm, Delayed ACK, and the 40ms Latency Cliff

## Nagle's Algorithm (RFC 896)
Designed to prevent congestion from millions of tiny packets (e.g. 1-byte Telnet keystrokes wrapped in 40-byte TCP/IP headers):
- Invariant: If there is unacknowledged data inflight (packets sent without receiving ACK), buffer outgoing data until:
  1. A full MSS (Maximum Segment Size, typically 1460 bytes) accumulates, OR
  2. All previously sent packets have been acknowledged (`ACK` received).

## TCP Delayed ACK (RFC 1122)
Servers delay sending standalone ACK packets by up to 40ms - 200ms in hope of piggybacking the ACK onto an application response packet.

## The Pathological 40ms Deadlock
When an application uses standard request/response framing (e.g. write header, write body):
1. Client sends header. Packet is small ($< \text{MSS}$). Sent immediately because no data is inflight.
2. Client sends body. Nagle buffers the body because the header packet is unacknowledged!
3. Server receives header, expects body before sending response. Waits for Delayed ACK timer (40ms).
4. System idles for exactly 40ms until the server's delayed ACK timer expires.
- **Fix**: Disable Nagle's algorithm for interactive low-latency sockets:
  ```c
  int flag = 1;
  setsockopt(sock_fd, IPPROTO_TCP, TCP_NODELAY, (char *)&flag, sizeof(int));
  ```
