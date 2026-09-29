# Hazard Pointers: Safe Memory Reclamation

## Problem Statement
In lock-free data structures, after a node is logically detached from a collection, other concurrent reader threads might still be dereferencing its pointers. Prematurely calling `free()` causes use-after-free corruption.

## Protocol Invariants
1. Each reader thread possesses a finite set of hazard pointer slots (typically 2 - 4 pointers).
2. Before reading a node, the thread publishes the address into its hazard pointer slot and re-validates that the node is still attached.
3. Deleting threads place detached nodes into a private retired list.
4. When the retired list exceeds a threshold R, the deleting thread scans all global hazard pointer arrays. Only nodes NOT protected by any hazard pointer are passed to `free()`.
