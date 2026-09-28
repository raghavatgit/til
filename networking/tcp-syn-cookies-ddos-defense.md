# TCP SYN Cookies: DDoS Defense Mechanics

## The SYN Flood Attack
During standard TCP 3-way handshakes, when a SYN arrives, the server allocates a Transmission Control Block (TCB) in the SYN backlog queue and sends SYN-ACK. Flooding spoofed SYNs exhausts kernel memory, rejecting legitimate clients.

## SYN Cookie Solution
When `tcp_syncookies = 1` and backlog overflows:
1. Server does NOT allocate state in the SYN backlog.
2. It encodes connection state into the initial sequence number (ISN):
   - Top 5 bits: `t mod 32` (timestamp counter).
   - Next 3 bits: MSS index.
   - Bottom 24 bits: `HMAC-SHA1(client_ip, client_port, server_ip, server_port, secret, timestamp)`.
3. When client sends ACK (`ack_seq = cookie + 1`), server recomputes the hash and validates validity without ever allocating unacknowledged TCB state.
