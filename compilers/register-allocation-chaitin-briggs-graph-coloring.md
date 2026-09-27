# Chaitin-Briggs Graph Coloring Register Allocation

## Interference Graphs & K-Coloring
Compilers represent program variable lifetimes as live ranges. An Interference Graph is constructed where nodes represent live ranges and edges connect ranges that overlap simultaneously.

Allocating `K` physical CPU registers is equivalent to solving the NP-complete Graph `K`-Coloring problem.

---

## Chaitin-Briggs Simplification Algorithm
1. **Simplify:** Find node `V` with degree `< K`. Remove it and push to stack (it can always be colored once neighbors are colored).
2. **Spill Decision:** If all remaining nodes have degree `>= K`, choose a high-cost candidate to spill to stack memory.
3. **Select:** Pop nodes from stack and assign physical registers.
