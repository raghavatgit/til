# Vector Clocks and Causality Tracking

## Definition
For a system of N nodes, each node maintains a vector V of size N:
- Local Event: Node i increments V[i].
- Message Send: Node i attaches its vector V to the payload.
- Message Receive: Node j updates its vector: V_j[k] = max(V_j[k], V_msg[k]) for all k, then increments V_j[j].

## Causal Ordering
Event A happened before Event B (A -> B) if and only if:
V_A[k] <= V_B[k] for all k, and V_A != V_B.
If neither A -> B nor B -> A, the events are concurrent (conflict detected).
