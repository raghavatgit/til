# Linux Kernel: Transparent Hugepages Direct Compaction and defrag Tuning

## The Synchronous Allocation Latency Stall
When `transparent_hugepage/enabled = always`:
- If physical memory is fragmented and no contiguous 2MB physical block is available:
- If `transparent_hugepage/defrag = always`:
  - The thread allocating memory blocks synchronously.
  - Kernel enters direct memory compaction, migrating physical pages across memory zones.
  - Generates severe latency spikes (50ms - 200ms) in database query paths (PostgreSQL, Redis).

## Production Recommendation
Tune defrag to prevent synchronous foreground allocation stalls:
```bash
echo madvise | sudo tee /sys/kernel/mm/transparent_hugepage/defrag
```
Only processes explicitly requesting hugepages via `madvise(MADV_HUGEPAGE)` trigger background compaction (`khugepaged`), insulating normal memory requests from latency cliffs.
