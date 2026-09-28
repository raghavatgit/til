# QUIC and HTTP/3: Solving Head-of-Line Blocking

## Limitations of HTTP/2 over TCP
HTTP/2 introduced logical streams over a single TCP connection. However, if a single packet is dropped on the wire, TCP halts delivery of all subsequent bytes across all streams until the missing packet is retransmitted. This is transport-layer Head-of-Line (HoL) blocking.

## The QUIC Architecture
- Runs over UDP.
- Implements encryption (TLS 1.3) directly in the transport framing.
- Streams are first-class transport entities: Packet loss on Stream A stalls only Stream A; Stream B and C continue uninterrupted.
- **Connection Migration**: Connections identified by Connection IDs (CID) rather than IP 4-tuples, surviving Wi-Fi to cellular handovers without disconnection.
