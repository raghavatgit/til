# Distributed Systems: Vector Clocks and Causal Consistency

## The Limit of Lamport Timestamps
Leslie Lamport's logical clocks establish a partial order:
- If event $A$ happened before event $B$ ($A \to B$), then $L(A) < L(B)$.
- **The Converse is False**: If $L(A) < L(B)$, we cannot infer whether $A \to B$ or if $A$ and $B$ were concurrent ($A \parallel B$).

## Vector Clock Formulation
In a cluster of $N$ nodes, each node maintains a vector clock $V$ of size $N$:
1. Node $i$ increments its own entry $V_i[i]$ before executing an event.
2. When transmitting a message, node $i$ attaches its entire vector clock $V_i$.
3. Upon receiving message with vector $V_{msg}$, node $j$ updates:
   $V_j[k] = \max(V_j[k], V_{msg}[k])$ for all $k$, then increments $V_j[j]$.

## Detecting Concurrency
Given two event vector timestamps $V_A$ and $V_B$:
- $V_A < V_B$ if and only if $\forall k, V_A[k] \le V_B[k]$ and $\exists k, V_A[k] < V_B[k]$.
- If neither $V_A \le V_B$ nor $V_B \le V_A$, the events are **concurrent** ($V_A \parallel V_B$).
Used in Dynamo-style distributed key-value databases (Cassandra, Riak) to trigger sibling resolution upon conflicting concurrent writes.
