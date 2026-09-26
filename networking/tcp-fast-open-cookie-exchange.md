# TCP Fast Open (TFO, RFC 7413)

## Eliminating Handshake Round-Trips
Standard TCP requires a full 3-way handshake (1 RTT) before client payload data can be transmitted. TCP Fast Open allows data to be embedded directly inside the initial `SYN` packet.

---

## TFO Security Token Flow
1. **Initial Request:** Client sends a `SYN` with a TFO Cookie Request option.
2. **Server Grant:** Server generates an encrypted cookie (AES-128 over client IP + secret key) and responds in `SYN-ACK`.
3. **Subsequent Connections:** Client sends `SYN` containing the cached TFO Cookie and initial application payload (e.g. TLS ClientHello).
4. **Instant Processing:** Server validates cookie and passes payload to socket read buffer before completing handshake, saving a full round-trip time.
