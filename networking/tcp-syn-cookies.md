# TCP SYN Cookies Defense Architecture

## Problem Statement
In a SYN flood attack, attackers send millions of spoofed SYN packets. The server allocates a `struct request_sock` in the SYN backlog queue for each packet, rapidly exhausting memory.

## Stateless Solution
When the SYN queue overflows, the server enables SYN cookies:
1. Zero Memory Allocation: The server does not allocate socket state.
2. Sequence Encoding: The server encodes timestamp, MSS index, and a cryptographic MAC of `(src_ip, dst_ip, src_port, dst_port, secret)` into the 32-bit initial sequence number (ISN).
3. Verification: When the client responds with ACK (`seq = ISN + 1`), the server recomputes the MAC and instantiates the connection only if valid.
