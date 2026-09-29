# Static Single Assignment (SSA) and Phi-Nodes

## Definition
A program is in SSA form if each variable is assigned a value exactly once, and every variable use is dominated by its definition.

## Phi-Node Placement
When two control-flow paths merge (e.g. after an `if-else` branch), a phi-node select value depends on which predecessor basic block executed:
`x_3 = phi [x_1, Block_True], [x_2, Block_False]`
Phi-nodes are placed at the iterated dominance frontier (IDF) of variable definition nodes.
