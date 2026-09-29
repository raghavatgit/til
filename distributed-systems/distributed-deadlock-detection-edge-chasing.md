# Distributed Deadlock Detection: Edge-Chasing

## The Global Wait-For Graph Problem
Maintaining a centralized Wait-For Graph across thousands of database nodes is a scalability bottleneck.

## Chandy-Misra-Haas Algorithm
When process $P_i$ blocks waiting for a resource held by $P_j$:
1. $P_i$ emits a probe message: `probe(initiator, sender, receiver)`.
2. $P_j$ propagates the probe to all processes it is waiting on.
3. If the probe message circles back to the initiator ($P_i$ receives `probe(P_i, ...)`), a distributed directed cycle exists: distributed deadlock is detected and resolved by aborting the transaction.
