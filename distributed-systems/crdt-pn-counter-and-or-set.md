# Conflict-free Replicated Data Types (CRDTs)

## Mathematical Foundation
State-based CRDTs (CvRDT) form a bounded semi-lattice with a merge operator $\sqcup$ satisfying:
1. **Commutativity**: $x \sqcup y = y \sqcup x$
2. **Associativity**: $(x \sqcup y) \sqcup z = x \sqcup (y \sqcup z)$
3. **Idempotence**: $x \sqcup x = x$

Any replicas merging states eventually converge to the identical state without central coordination.

## PN-Counter (Positive-Negative Counter)
Composed of two grow-only counters ($P$ for increments, $N$ for decrements):
- Value: $\sum P - \sum N$
- Merge: $P_{merged}[i] = \max(P_1[i], P_2[i])$, $N_{merged}[i] = \max(N_1[i], N_2[i])$.
