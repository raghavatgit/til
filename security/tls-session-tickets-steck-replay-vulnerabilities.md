# TLS 1.3 0-RTT Resumption & Replay Attacks

## The Performance Advantage of 0-RTT
In TLS 1.3, early data (0-RTT) allows clients to send HTTP requests in the initial `ClientHello` before receiving the server handshake response, eliminating connection latency.

---

## The Replay Attack Vulnerability
Because early data is sent before server authentication is finalized, a network adversary can capture the 0-RTT packet and replay it multiple times to the server.

### Mitigation Strategies
* **Client Anti-Replay Tokens:** Server tracks unique identifiers in a sliding window memory cache.
* **Idempotent Requests Only:** Restrict 0-RTT strictly to safe GET requests; prohibit non-idempotent POST/PUT operations.
