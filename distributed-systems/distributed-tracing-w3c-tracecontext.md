# Distributed Tracing: W3C TraceContext Specification

## Problem
In a microservices ecosystem, a single client interaction spans dozens of downstream services. Without contextual propagation, diagnosing end-to-end latency bottlenecks is intractable.

## W3C TraceContext Standard (RFC)
Propagates state across HTTP headers:
- `traceparent`: `00-{trace-id}-{parent-id}-{trace-flags}`
  - Version: `00`
  - Trace ID (32 hex characters / 16 bytes): Globally unique identifier for entire transaction.
  - Parent ID / Span ID (16 hex characters / 8 bytes): Identifier for calling segment.
  - Trace Flags (8 bits): `01` enables recording/sampling.
- `tracestate`: Opaque vendor-specific key-value list for tracing system interoperability.
