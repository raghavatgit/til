# Linux Page Cache Dirty Writeback Mechanics

## Problem Space: Buffered I/O Latency Spikes
When processes write data via standard `write()` system calls, the Linux kernel buffers modified pages in the Page Cache as "dirty pages" without immediately flushing to block devices. While this achieves near-instant write throughput, unconstrained dirty page accumulation causes major background I/O storms, stalls, and tail latency spikes when flusher threads write gigabytes to disk concurrently.

---

## Kernel Tuning Knobs & Thresholds

The kernel balances asynchronous writeback via `vm` sysctl parameters:

1. **`vm.dirty_background_ratio` / `vm.dirty_background_bytes`**:
   - Threshold at which kernel flusher threads (`kworker/flush`) awaken in the background to start cleaning dirty pages.
   - Default is typically 10% of total system RAM.
   - Writes continue asynchronously without blocking user processes.

2. **`vm.dirty_ratio` / `vm.dirty_bytes`**:
   - Hard upper limit. When dirty memory reaches this percentage (typically 20%), any process calling `write()` is synchronously blocked and forced to perform writeback itself.
   - Induces catastrophic latency spikes on database engines and message brokers.

3. **`vm.dirty_expire_centisecs`**:
   - Maximum age (in 1/100ths of a second) a page can remain dirty before writeback is mandatory (default 3000 = 30 seconds).

4. **`vm.dirty_writeback_centisecs`**:
   - Polling interval for flusher thread wakeups (default 500 = 5 seconds).

---

## Production Recommendation for Low-Latency Systems

On high-memory servers (e.g. 128 GB+ RAM running RocksDB, Postgres, or Kafka), default percentage ratios cause 12-25 GB of dirty memory to queue up before flushing. Switch from ratios to absolute byte limits:

```bash
# Flush early and continuously in small increments
sudo sysctl -w vm.dirty_background_bytes=134217728   # 128 MB
sudo sysctl -w vm.dirty_bytes=536870912              # 512 MB
sudo sysctl -w vm.dirty_writeback_centisecs=100       # Check every 1 second
```
