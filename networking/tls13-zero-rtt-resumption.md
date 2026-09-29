# TLS 1.3 0-RTT Session Resumption

## Handshake Comparison
- TLS 1.2: 2-RTT initial handshake, 1-RTT resumption.
- TLS 1.3: 1-RTT initial handshake, 0-RTT session resumption via Pre-Shared Keys (PSK).

## Replay Attack Vulnerability
Because 0-RTT early data is sent in the first flight before server certificate verification, an eavesdropper can intercept and re-transmit the exact encrypted packet.
Mitigation: Servers must restrict 0-RTT data exclusively to idempotent requests (e.g. GET requests with single-use replay caches or client timestamp windows).
