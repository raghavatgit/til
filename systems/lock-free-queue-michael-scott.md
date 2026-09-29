# Michael-Scott Lock-Free Concurrent Queue

The Michael-Scott queue is a non-blocking FIFO queue utilizing single-word Compare-And-Swap (CAS).

## Invariants
1. `head` always points to a sentinel dummy node or valid queue head.
2. `tail` points to the last node or second-to-last node (lagging tail).
3. Enqueue uses two CAS steps: link new node to `tail->next`, then swing `tail` to new node.
