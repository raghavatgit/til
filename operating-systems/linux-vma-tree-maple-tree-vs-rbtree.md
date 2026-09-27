# Linux VMA Tracking: Maple Tree vs Red-Black Tree

## Red-Black Tree Limitations in VMA Management
Historically, each process's Virtual Memory Areas (VMAs) were indexed in a red-black tree with an augmented doubly linked list. However, modern multi-threaded applications map tens of thousands of VMAs. Traversing RB-trees under `mmap_lock` caused heavy lock contention and cache-line misses during page faults.

---

## The Maple Tree (Linux 6.1+)
The Maple Tree is a B-tree variant optimized for storing non-overlapping ranges (intervals).
* **Cache-Aligned Node Layout:** 64-byte nodes match CPU cache line boundaries.
* **RCU-Safe Traversal:** Allows lockless read-side lookups of memory ranges without holding write locks.
* **Range Concatenation:** Merges adjacent memory allocations cleanly without balance tree rotations.
