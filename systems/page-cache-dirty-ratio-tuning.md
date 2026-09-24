# Linux Page Cache: Dirty Ratio Writeback Dynamics

## Key Kernel Tunables
- `/proc/sys/vm/dirty_background_ratio`: Percentage of system memory containing dirty pages at which background kernel threads (`flusher`) begin writing to disk.
- `/proc/sys/vm/dirty_ratio`: Maximum percentage of memory containing dirty pages. Once reached, producing processes are blocked from allocating memory and forced to synchronously write pages to disk.

## Latency Spike Prevention
Default values (e.g., 20% on a 256GB server = 51GB dirty data) cause seconds-long I/O stalls when flusher flushes all at once.
Best practice for low-latency databases:
```bash
sysctl -w vm.dirty_background_bytes=268435456 # 256MB
sysctl -w vm.dirty_bytes=1073741824           # 1GB
```
