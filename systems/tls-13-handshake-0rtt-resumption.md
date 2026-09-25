# Cryptography: TLS 1.3 Handshake and 0-RTT Early Data

## Latency Optimization
- **TLS 1.2**: Required 2 full round-trips (2-RTT) before application data could be sent.
- **TLS 1.3 Full Handshake**: Reduced to 1-RTT by sending Diffie-Hellman key share in `ClientHello`.
- **TLS 1.3 Session Resumption (0-RTT)**: Reconnect clients send encrypted application data in the very first flight alongside `ClientHello`.

## The 0-RTT Replay Attack Risk
Because 0-RTT data is encrypted with a pre-shared key (PSK) before the server confirms ephemeral key shares:
An attacker intercepting the initial 0-RTT packet can replay it against the server multiple times.
Mitigation:
- Only idempotent HTTP requests (`GET`, `HEAD`) should ever be permitted over 0-RTT early data.
- State-changing actions (`POST /transfer`, `PUT`) must be strictly blocked until 1-RTT handshake completion.
