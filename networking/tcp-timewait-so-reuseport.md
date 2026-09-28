# TCP TIME_WAIT and SO_REUSEPORT

## Purpose of TIME_WAIT (2 * MSL)
The endpoint that initiates active close enters `TIME_WAIT` for 2 Maximum Segment Lifetimes (typically 60s):
1. Ensures lingering duplicate segments expire in the network before a new connection reuses the same 4-tuple.
2. Ensures the final ACK is acknowledged; if dropped, the peer will retransmit FIN, which can be re-ACKed.

## SO_REUSEPORT Socket Option
Allows multiple independent threads or processes to bind to the exact same IP and port:
- Kernel distributes incoming SYN packets across listening sockets via four-tuple hashing.
- Eliminates the accept-mutex bottleneck and thundering-herd issues in multi-core network servers.
