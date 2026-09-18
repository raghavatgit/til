# Compilers: Static Single Assignment (SSA) and Dominator Trees

## The Purpose of SSA Form
In classical intermediate representations (IR), variables can be assigned multiple times:
```
x = 1;
...
x = 2;
```
Static Single Assignment (SSA) guarantees every variable is assigned exactly once. Subscripted variables (`x_1`, `x_2`) isolate definition-use chains:
- Simplifies dead code elimination (DCE).
- Enables global value numbering (GVN) and sparse conditional constant propagation (SCCP).

## The $\phi$ (Phi) Node Function
When two execution paths merge at a control flow join point:
```
if (condition) {
    x_1 = 10;
} else {
    x_2 = 20;
}
x_3 = phi(x_1, x_2);
```
The $\phi$-node dynamically selects the value depending on which basic block preceded execution.

## Dominance Frontiers and Minimal SSA
A node $d$ dominates node $n$ ($d \text{ dom } n$) if every execution path from entry to $n$ must pass through $d$:
- **Dominance Frontier ($DF$)**: The set of nodes where dominance ceases.
- Cytron's algorithm places $\phi$-nodes exclusively at the dominance frontiers of basic blocks containing variable definitions, producing **Minimal SSA**.
