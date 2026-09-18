# Networking: TLS 1.3 Latency Optimization and 0-RTT Replay Protection

## The TLS 1.2 Handshake (2-RTT)
1. Client sends `ClientHello`.
2. Server responds with `ServerHello`, Certificate, and Key Exchange parameters.
3. Client verifies certificate, computes premaster secret, sends Key Exchange and `Finished`.
4. Server responds with `Finished`.
- **Latency**: 2 full network round-trips before application data (HTTP request) can be sent.

## The TLS 1.3 Streamlined Handshake (1-RTT)
RFC 8446 removes legacy key exchange methods (RSA key exchange without forward secrecy) and mandates Diffie-Hellman (ECDHE):
1. Client assumes server supports common ECDHE groups and speculatively sends its key share in `ClientHello`.
2. Server immediately responds with its key share and encrypted certificate in `ServerHello`.
3. Both parties derive session keys. Application data follows immediately.
- **Latency**: Reduced by 50% to 1-RTT.

## 0-RTT Early Data and Anti-Replay
On resumed connections, clients can send early application data alongside `ClientHello`:
- **Security Invariant**: 0-RTT data is susceptible to network replay attacks because an attacker can capture and retransmit the packet.
- **Mitigation**: 0-RTT should strictly be restricted to idempotent HTTP methods (`GET`, `HEAD`).
