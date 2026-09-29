# QUIC Stream Multiplexing and Head-of-Line Blocking

## The HTTP/2 over TCP Bottleneck
In HTTP/2, multiple application streams are multiplexed over a single TCP connection. If a single TCP segment drops, TCP stops all stream delivery until the missing packet is retransmitted (TCP Head-of-Line blocking).

## QUIC Solution
QUIC operates over UDP. Each QUIC stream possesses independent packet framing and offset tracking. If a packet on Stream 3 drops, Stream 1 and Stream 2 continue delivering bytes to userspace without delay.
