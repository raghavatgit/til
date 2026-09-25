# Distributed Systems: Conflict-Free Replicated Data Types (CRDTs)

## Strong Eventual Consistency
CRDTs guarantee that as long as all replicas have received the same set of updates, they will deterministically converge to the exact same state without requiring centralized coordination.

## 1. State-Based CRDTs (CvRDT)
- Replicas synchronize by transmitting their full internal states.
- Merge operation forms a Join-Semilattice:
  - Commutative: $A \sqcup B = B \sqcup A$
  - Associative: $(A \sqcup B) \sqcup C = A \sqcup (B \sqcup C)$
  - Idempotent: $A \sqcup A = A$
- Example: G-Counter, PN-Counter, Observed-Remove Set (OR-Set).

## 2. Operation-Based CRDTs (CmRDT)
- Replicas transmit operations rather than full state.
- Requires reliable causal delivery from the transport layer.
- Operations must commute with all concurrent operations.
