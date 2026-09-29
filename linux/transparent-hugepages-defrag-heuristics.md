# Transparent HugePages (THP) Compaction and Defrag Modes

## The Defrag Problem
When physical memory becomes fragmented, allocating a contiguous 2 MB hugepage requires the kernel memory management subsystem to migrate smaller 4 KB pages into contiguous frames.

## Allocation Modes (`/sys/kernel/mm/transparent_hugepage/defrag`)
1. **always**: The allocating thread halts in direct compaction until a 2 MB frame is assembled. Induces latency spikes up to hundreds of milliseconds.
2. **defer**: Allocation immediately falls back to standard 4 KB pages, while background daemon `kcompactd` defragments memory asynchronously.
3. **madvise**: Compaction occurs exclusively within virtual memory regions annotated with `madvise(MADV_HUGEPAGE)`.
