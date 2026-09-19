# Distributed Systems: Conflict-Free Replicated Data Types (CRDTs)

## The Core Invariant: Eventual Consistency Without Central Locks
CAP theorem dictates that partitioned systems cannot guarantee both consistency and availability.
CRDTs (Shapiro et al.) allow concurrent, asynchronous modifications on disconnected replicas and guarantee mathematically proven convergence to identical states once messages replicate.

## State-Based CRDTs (CvRDT)
Replicas synchronize by transmitting their entire state:
- The state space forms a **Bounded Semi-Lattice**:
  - A partial order $\le$ representing information growth.
  - A join operator $\sqcup$ (Least Upper Bound) that is:
    1. **Commutative**: $A \sqcup B = B \sqcup A$
    2. **Associative**: $(A \sqcup B) \sqcup C = A \sqcup (B \sqcup C)$
    3. **Idempotent**: $A \sqcup A = A$
- Guarantees that message reordering, duplication, or delayed network arrival always resolves to the same value.

## Canonical CRDT Types
1. **PN-Counter (Positive-Negative Counter)**: Two Grow-Only counters tracking increments and decrements. Value is $\sum P - \sum N$.
2. **LWW-Element-Set (Last-Write-Wins Element Set)**: Elements stored with logical timestamps. Conflicts resolved deterministically by comparing timestamps.
3. **RGA / Yjs / Automerge**: Replicated sequence trees supporting collaborative real-time text editing with character insertion IDs.
