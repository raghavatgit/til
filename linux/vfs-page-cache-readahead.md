# Linux VFS Adaptive Readahead Mechanics

The Linux kernel implements sequential page cache readahead through dynamic window scaling managed in `mm/readahead.c`.

## Invariants
1. `ra->size`: Current readahead window in pages.
2. `ra->async_size`: Lookahead watermark triggering asynchronous background I/O before application read exhaustion.
3. On sequential page hits, window size doubles up to `max_readahead_kb` (typically 128KB - 256KB).
4. Random I/O cache misses collapse the window to zero, switching to synchronous single-page faults.

## Sysctl Tunables
```bash
# Inspect readahead size for block device sda
cat /sys/block/sda/queue/read_ahead_kb

# Set readahead to 512KB for streaming analytical workloads
echo 512 > /sys/block/sda/queue/read_ahead_kb
```
