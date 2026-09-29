# gRPC HTTP/2 Keepalive and Idle Timeout Architecture

## Problem Statement
Cloud load balancers (e.g. AWS NLB, GCP Cloud Load Balancing) drop idle TCP connections after 300 - 350 seconds without sending TCP FIN/RST packets. Clients attempting to reuse the dead connection experience connection reset errors.

## Mitigation
Clients configure active HTTP/2 keepalive PING frames every 30 seconds with 10-second ACK timeouts to keep NAT mappings alive.
