# jemalloc Arenas & Thread Cache (tcache) Architecture

## Motivation & Lock Contention in General Allocators
Traditional monolithic allocators (like legacy `glibc` `ptmalloc`) bottleneck under high concurrency due to global heap mutex contention. `jemalloc` eliminates this bottleneck by sharding heap allocations across independent arenas and per-thread private caches.

---

## Core Structural Hierarchy

1. **Thread Cache (`tcache`)**:
   - Each thread maintains a thread-local bin of pre-allocated small chunks (`<= 14 KB`).
   - Small allocations and deallocations execute lock-free with zero inter-thread synchronization overhead.
   - Flushes back to the arena using decay algorithms when thread cache fills or thread exits.

2. **Arenas**:
   - The heap is partitioned into `N = 4 * n_cpus` independent arenas.
   - Threads are assigned round-robin or affinity-mapped to arenas to distribute remaining lock contention.
   - Arenas manage slabs, extents, and virtual memory chunk boundaries.

3. **Extents & Chunk Allocation**:
   - Large memory allocations (`> 14 KB` and `<= 2 MB`) bypass `tcache` and go directly to arena extent trees.
   - Huge allocations (`> 2 MB`) use direct `mmap` anonymous mappings with explicit virtual address alignment.

---

## Memory Purging & Decay Strategy
- `jemalloc` replaces aggressive `madvise(MADV_DONTNEED)` sweeps with time-decay based page reclamation (`dirty_decay_ms` and `muzzy_decay_ms`).
- Smoothes latency spikes in high-throughput database services (e.g., Redis, TiDB, ClickHouse).
