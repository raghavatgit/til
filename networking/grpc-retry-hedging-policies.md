# gRPC Request Retries and Hedging Policies

## Service Config Specification
Configured declaratively in JSON service configurations:
- **Retries**: Reissues failed RPCs upon receiving specified status codes (`UNAVAILABLE`, `RESOURCE_EXHAUSTED`).
- **Backoff**: Randomized exponential backoff (`initialBackoff`, `maxBackoff`, `backoffMultiplier = 1.5`).

## Request Hedging
Hedging mitigates tail latency by issuing duplicate requests to secondary server replicas before the initial request has returned an error:
1. Client sends RPC to Server A.
2. If no response arrives within `hedgingDelay` (e.g., 20ms), Client sends duplicate RPC to Server B.
3. Whichever server returns headers first is accepted; the other is cancelled immediately via `CANCEL` frame.
