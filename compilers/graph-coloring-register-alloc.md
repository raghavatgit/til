# Chaitin-Briggs Register Allocation via Graph Coloring

## Interference Graph
Vertices represent variable live ranges. An edge connects two vertices if their live ranges overlap simultaneously.

## Kempe Heuristic
Given K physical CPU registers:
1. Simplify: Find a node with degree < K, push it to a stack, and remove it from the graph.
2. Spill: If all nodes have degree >= K, select a candidate to spill to stack memory.
3. Select: Pop nodes from the stack and assign an available color (register) that does not conflict with colored neighbors.
