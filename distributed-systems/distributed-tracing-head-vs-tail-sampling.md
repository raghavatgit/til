# Distributed Tracing: Head-Based vs Tail-Based Sampling

Modern telemetry collectors manage petabyte-scale trace volumes by trading sampling decisions against latency and trace completeness.

## Comparison
| Metric | Head-Based Sampling | Tail-Based Sampling |
| :--- | :--- | :--- |
| Decision Point | Root service at trace creation | Trace collector proxy cluster |
| Storage Cost | Minimal (discarded early) | Requires collector memory buffer |
| Anomaly Retention | Random chance (blind to errors) | 100% of errors and p99 traces |
| Implementation | W3C traceparent flag `01` | Envoy / OpenTelemetry Collector |
