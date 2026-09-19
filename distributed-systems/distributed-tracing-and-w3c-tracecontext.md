# Distributed Systems: Distributed Tracing and W3C TraceContext

## The Propagation Challenge
In microservices architectures, a single user request can fan out across dozens of RPC services.
To reconstruct the end-to-end distributed execution trace, context must propagate across network boundaries.

## W3C TraceContext Standard
Standard HTTP header specification:
`traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01`
1. `version`: `00` (current format version).
2. `trace-id`: 16-byte hex string globally identifying the end-to-end transaction.
3. `parent-id` / `span-id`: 8-byte hex string identifying the immediate caller span.
4. `trace-flags`: 8-bit field. Bit 0 indicates whether the trace is sampled (`01` = sampled for persistent storage).

Used ubiquitously across OpenTelemetry (OTel), Jaeger, and Cloud Trace.
