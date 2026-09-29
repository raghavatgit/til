# Resource Hints: Preconnect, DNS-Prefetch, and Modulepreload

## Hints Comparison
- `dns-prefetch`: Resolves domain IP ahead of time (saves ~30-100ms DNS lookup).
- `preconnect`: Performs DNS lookup, TCP handshake, and TLS negotiation (saves full 1-2 RTT).
- `modulepreload`: Fetches, parses, and compiles ES modules into the module map before execution.
