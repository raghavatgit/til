# W3C Distributed Trace Context

## Traceparent Header Format
`traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01`

Where:
- Version: `00`
- Trace ID: 16-byte hex string (identifies distributed transaction)
- Parent ID: 8-byte hex string (identifies caller span)
- Trace Flags: `01` (indicates sampled)
