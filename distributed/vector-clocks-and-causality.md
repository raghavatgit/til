# Distributed Causality: Vector Clocks

## Representation
In a system of $N$ nodes, each node $i$ maintains a vector clock $V$ of size $N$:
1. Before event creation on node $i$: $V[i] = V[i] + 1$.
2. Message sent from node $i$ carries its current vector clock $V$.
3. Upon receiving message with vector $V_{msg}$, node $j$ updates:
$$V[k] = \max(V[k], V_{msg}[k]) \quad \forall k$$
and increments $V[j] = V[j] + 1$.

## Causality Ordering
- $V_A < V_B$ if $V_A[k] \le V_B[k]$ for all $k$ and $\exists k$ where $V_A[k] < V_B[k]$ (A happened before B).
- Otherwise, events are concurrent ($A \parallel B$), signaling conflict to be resolved by application.
