# Linux Memory Compaction and Zone Watermarks

Linux memory compaction mitigates external fragmentation without reclaiming page cache entries by migrating movable pages to high-order page blocks.

## Mechanism
- **Free Scanner**: Scans bottom-up from zone start to find contiguous unallocated pages.
- **Migration Scanner**: Scans top-down from zone end to isolate movable page structs (`PG_movable`).
- When scanners meet, memory compaction completes a pass over the zone.

## Watermarks
- `WMARK_MIN`: Direct reclaim and compaction invoked synchronously if alloc falls below this threshold.
- `WMARK_LOW`: Background `kswapd` wakes to scan LRU lists and trigger compaction.
- `WMARK_HIGH`: `kswapd` goes to sleep when free pages cross this boundary.
