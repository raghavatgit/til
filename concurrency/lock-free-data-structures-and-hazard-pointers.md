# Concurrency: Lock-Free Safe Memory Reclamation via Hazard Pointers

## The Problem: Reclamation in Lock-Free Structures
In a lock-free queue or stack (e.g. Treiber Stack), popping a node detaches it via `compare_and_swap`:
- Another concurrent reader might have loaded the node pointer and paused before reading its fields.
- If the popping thread immediately calls `free()` or drops the node, the paused reader triggers a use-after-free or read from unmapped memory.

## Hazard Pointer Mechanism
Hazard pointers provide safe memory reclamation without lock contention:
1. Each reader thread owns a small array of **hazard pointers** (typically 1 or 2 pointers).
2. Before dereferencing a shared node, a thread writes the node's memory address into its active hazard pointer.
3. It re-verifies that the node is still the current head/tail in the shared structure (to prevent reading an already-freed pointer).
4. When a thread pops a node, it places it in a thread-local retirement list instead of freeing it.
5. When the retirement list reaches a threshold $R$:
   - The thread scans all hazard pointers of all threads.
   - Nodes present in any thread's hazard pointer list are retained.
   - Unreferenced nodes are safely freed.
6. Guarantees deterministic $O(1)$ reclamation overhead and bounds memory reclamation latency.
