# Networking: TCP SYN Flood Mitigation via Cryptographic SYN Cookies

## The Vulnerability: Half-Open Connection Table Exhaustion
In standard TCP 3-way handshakes:
1. Client sends `SYN`.
2. Server allocates a `struct request_sock` in the kernel SYN backlog queue and responds with `SYN-ACK`.
3. An attacker flooding millions of spoofed `SYN` packets fills the backlog queue, causing the server to drop legitimate incoming connection requests.

## SYN Cookies Architecture (D. J. Bernstein)
When the SYN backlog queue overflows, the kernel switches to stateless SYN cookies:
- The server does **not** allocate memory in the backlog queue.
- It encodes connection state directly into the 32-bit Initial Sequence Number (ISN) of the `SYN-ACK`:
  - Top 5 bits: Slow counter incremented every 64 seconds.
  - Middle 3 bits: Encoded Maximum Segment Size (MSS) choice.
  - Bottom 24 bits: Truncated HMAC-SHA1 computed over client IP, client port, server IP, server port, and secret key.
- When the legitimate client responds with `ACK`, the server reconstructs the secret hash from `ack_seq - 1`. If valid, it allocates the full TCP socket connection.
- Completely defeats half-open memory exhaustion attacks with zero client protocol changes.
