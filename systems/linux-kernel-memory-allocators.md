# Linux Kernel Allocators: SLAB vs SLUB vs SLOB

## The Problem
Page allocator (`alloc_pages`, buddy system) operates at page granularity (4KB). Allocating small kernel structs (e.g. `task_struct`, `inode`) directly from pages causes catastrophic internal fragmentation.

## Allocator Evolution
1. **SLAB (Classic Bonwick)**: Maintains queues of objects (full, partial, empty). High bookkeeping overhead per CPU cache.
2. **SLUB (Default Modern Linux)**: Eliminates complex queues. Embeds freelist pointers directly inside idle objects. Minimizes metadata footprint and scales across NUMA nodes.
3. **SLOB (Simple List of Blocks)**: First-fit linked list allocator optimized strictly for tiny embedded systems with severe RAM constraints.
