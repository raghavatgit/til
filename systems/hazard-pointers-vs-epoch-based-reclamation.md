# Lock-Free Memory Reclamation: Hazard Pointers vs Epoch-Based Reclamation

## The ABA Problem & Use-After-Free in Concurrency
In lock-free data structures using Compare-And-Swap (CAS), a thread reading a pointer cannot simply `free()` the memory node if another thread might concurrently read or traverse it.

---

## 1. Hazard Pointers (Maged Michael)
- Every reader thread publishes a small set of "Hazard Pointers" (globally visible atomic pointers) indicating the nodes it is currently reading.
- Before freeing a node:
  - The node is unlinked from the global data structure.
  - The reclaiming thread scans all active hazard pointers.
  - If any thread holds a hazard pointer to the node, retirement is deferred.
  - If no hazard pointers reference the node, it is safely deleted.

**Characteristics**:
- Strict upper bound on un-reclaimed memory (`O(Threads * PointersPerThread)`).
- Reader overhead: atomic write barrier to publish hazard pointer before each node dereference.

---

## 2. Epoch-Based Reclamation (EBR / Crossbeam)
- Global atomic counter tracks the current Epoch (`0, 1, 2`).
- Reader threads register their local active epoch upon entering a critical section.
- Memory deleted in epoch `E` is deferred until all threads have advanced past epoch `E`.

**Characteristics**:
- Near-zero reader overhead (a single non-atomic or relaxed atomic counter read).
- Susceptible to stalled reclamation if a single thread sleeps or hangs in a critical section.
