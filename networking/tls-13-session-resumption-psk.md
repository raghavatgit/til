# TLS 1.3 Session Resumption and 0-RTT Replay Defense

## Pre-Shared Keys (PSK)
When a TLS 1.3 connection closes, the server issues a `NewSessionTicket` containing an encrypted state blob (PSK). On subsequent reconnections, the client presents the PSK in its `ClientHello`.

## 0-RTT Early Data
The client can send encrypted application data alongside the initial `ClientHello`, achieving zero network delay on page loads.

## Anti-Replay Mechanisms
Because 0-RTT data has no forward secrecy before the server handshake completes:
1. **Single-Use Ticket Cache**: Server tracks recently consumed ticket identifiers in Redis/memory.
2. **Client Hello Recording**: Server rejects duplicate ClientHello payloads within a freshness time window.
3. Safe HTTP Methods: Early data must only be permitted for idempotent requests (GET/HEAD), never POST/PUT.
