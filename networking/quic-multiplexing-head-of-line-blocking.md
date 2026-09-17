# QUIC Transport: Solving TCP Head-of-Line Blocking

## The Fundamental TCP Limitation
HTTP/2 introduced application-layer multiplexing over a single TCP connection, allowing concurrent requests and responses over virtual streams.
However, TCP is an in-order byte stream:
- If a single IP packet carrying data for Stream A is lost or delayed, the operating system's TCP receive buffer halts delivery of all subsequent packets.
- Streams B, C, and D are blocked waiting for the TCP retransmission of Stream A, even though their own packets have arrived safely at the network interface card (NIC).
- This is transport-layer Head-of-Line (HoL) blocking.

## The QUIC Architecture (RFC 9000)
QUIC runs on top of UDP and moves stream management directly into the transport layer:
1. **Independent Byte Streams**: Each QUIC stream possesses its own sequence offset numbering. Packet loss on Stream A only pauses Stream A; Streams B, C, and D proceed with zero delay.
2. **Integrated Encryption**: TLS 1.3 cryptographic handshake is merged with the transport handshake, achieving 0-RTT or 1-RTT connection setup.
3. **Connection Migration**: QUIC identifies connections via a 64-bit Connection ID (CID) rather than the standard IP:Port 4-tuple. If a mobile device switches from Wi-Fi to 5G cellular, the connection persists without renegotiating TCP or TLS sessions.
