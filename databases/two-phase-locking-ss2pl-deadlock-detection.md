# Concurrency Control: Strict Two-Phase Locking and Deadlock Detection

## Strict Two-Phase Locking (SS2PL)
- **Growing Phase**: Transaction acquires shared locks (S) for reading and exclusive locks (X) for writing. Cannot release any locks.
- **Shrinking Phase**: All locks are held until transaction commits or aborts, then released simultaneously.
- Guarantee: Eliminates cascading aborts and guarantees strict serializability.

## Deadlock Detection
- **Wait-For Graph (WFG)**: Directed graph where vertices represent transactions and edge $T_1 \to T_2$ indicates $T_1$ is waiting for a lock held by $T_2$.
- Deadlock exists if and only if the graph contains a directed cycle.
- Detection engine runs Tarjan or DFS cycle detection periodically; upon cycle detection, aborts the youngest or least expensive transaction.
