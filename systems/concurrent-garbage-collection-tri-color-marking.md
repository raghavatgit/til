# Concurrent Garbage Collection & The Tri-Color Abstraction

## The Core Problem of Concurrent GC
When a garbage collector runs concurrently with application mutator threads, mutators can alter object references while the collector is traversing the object graph, potentially hiding live objects from the mark phase.

---

## The Tri-Color Set Model

1. **White**: Unvisited objects (candidates for reclamation).
2. **Grey**: Visited objects whose children have not yet been traversed.
3. **Black**: Visited objects with all direct children discovered and marked.

### The Mutator Invariant Violation
A live object will be erroneously collected if and only if both conditions occur concurrently:
1. Mutator creates a reference from a **Black** object directly to a **White** object.
2. Mutator destroys all existing paths from **Grey** objects to that **White** object.

---

## Write Barrier Solutions

1. **Dijkstra Insertion Barrier**:
   - Intercepts pointer writes: whenever a reference `A -> B` is installed, shade `B` grey.
   - Prevents condition 1 by ensuring no direct black-to-white edge can exist without marking the target.

2. **Yuasa Deletion Barrier**:
   - Intercepts pointer deletions: whenever an old reference `A -> B` is overwritten, shade `B` grey.
   - Preserves snapshot-at-the-beginning invariants.
