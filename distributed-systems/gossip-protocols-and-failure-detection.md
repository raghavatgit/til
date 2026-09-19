# Distributed Systems: Gossip Protocols and Phi-Accrual Failure Detectors

## Epidemic Information Dissemination
In large-scale decentralized clusters (Cassandra, HashiCorp Consul, Amazon Dynamo):
- Centralized heartbeats introduce single points of failure and bottleneck scaling ($O(N)$ message load per node).
- Gossip protocols periodically select $k$ random peer nodes to exchange cluster membership state:
  - Dissemination converges exponentially: state reaches all $N$ nodes in $O(\log N)$ rounds.
  - Resilient to network packet loss and transient node disconnects.

## SWAN Protocol and Indirect Probing
Standard ping-ack failure detection suffers from false positives on localized network blips:
1. Node $A$ pings Node $B$. If no ACK within timeout:
2. Node $A$ requests $k$ random peer nodes to send indirect pings to $B$ (`ping-req`).
3. If any indirect ping succeeds, $B$ is deemed healthy.
4. Prevents false positive node ejections caused by a single degraded network path.

## $\phi$-Accrual Failure Detector (Hayashibara et al.)
Instead of binary alive/dead thresholds:
- Models heartbeat arrival intervals as a normal distribution.
- Computes suspicion metric $\phi = -\log_{10}(P_{\text{later}}(t - t_{\text{last}}))$.
- High $\phi$ dynamically triggers graduated responses (e.g. pause routing at $\phi > 8$, trigger partition failover at $\phi > 12$).
