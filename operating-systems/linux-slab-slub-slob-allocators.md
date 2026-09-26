# Linux SLAB, SLUB, and SLOB Kernel Allocators

## Overview: Eliminating Internal Fragmentation
The Linux page allocator operates on whole 4 KB pages. The slab allocator sits on top, servicing small kernel object allocations (`dentry`, `inode`, `sk_buff`) with zero internal fragmentation.

---

## Architectural Comparison
* **SLAB (Legacy):** High overhead queues, coloring for CPU L1 cache optimization, complex per-CPU object caches.
* **SLUB (Modern Default):** Removes complex queue structures. Uses page structs directly to track freelists, dramatically reducing memory metadata overhead on large NUMA servers.
* **SLOB (Simple List of Blocks):** Kmalloc space-optimized for embedded microcontrollers with tiny memory footprints (deprecated in modern kernels).
