# Hazard Pointers vs Epoch-Based Reclamation (EBR)

## The ABA Problem & Safe Memory Reclamation
In lock-free data structures, deleting a node cannot immediately free its memory (`free()`), as concurrent threads might currently hold references to it.

---

## Two Reclamation Philosophies
1. **Hazard Pointers:** Each reading thread publishes the exact memory pointer it is currently inspecting in a thread-local hazard pointer slot. Reclaiming threads verify no hazard pointer matches the address before deallocating.
2. **Epoch-Based Reclamation (EBR):** A global epoch counter advances periodically. Threads record their active epoch. Memory retired in epoch `E` is deferred until all threads have advanced to epoch `E + 2`.
