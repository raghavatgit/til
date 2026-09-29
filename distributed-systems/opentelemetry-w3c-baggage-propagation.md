# OpenTelemetry Baggage: W3C Baggage Specification

## Baggage vs TraceContext
- `traceparent`: Propagates span identifiers for distributed tracing.
- `baggage`: Propagates user-defined business context (e.g., `user_id`, `tenant_id`, `datacenter`) across asynchronous RPC boundaries without altering intermediate database schemas.

## Wire Format
`baggage: userId=alice,serverNode=dfw-01;qos=fast`
- Key-value pairs encoded in RFC 7230 headers.
- Propagated automatically through HTTP, gRPC, and Kafka metadata by OpenTelemetry context propagators.
