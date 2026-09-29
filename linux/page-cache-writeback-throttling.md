# Linux Page Cache: Dirty Memory Writeback Throttling

## Writeback Lifecycle
When an application calls `write()`, bytes are written to kernel page cache frames and marked "dirty".
- `flusher` background threads (per-backing-device `bdi_writeback`) flush pages to storage.

## Kernel Tunables
- `vm.dirty_background_ratio`: Percentage of total system memory at which background flushing begins.
- `vm.dirty_ratio`: Hard ceiling. If dirty memory reaches this percentage, subsequent user-space `write()` calls block synchronously while flushing pages to disk.
- Recommended database setting: Low absolute limits (`vm.dirty_bytes` = 256MB) to prevent I/O queue saturation.
