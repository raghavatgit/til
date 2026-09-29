# cgroups v2 Unified I/O Controller Architecture

cgroups v2 unifies block I/O resource allocation with memory writeback throttling, solving the writeback inversion bug in cgroups v1.

## Controllers
- `io.weight`: Proportional fair-share I/O scheduling via BFQ (1-10000).
- `io.max`: Absolute hard limits for IOPS and byte throughput per device (`rbps`, `wbps`, `riops`, `wiops`).
- `io.latency`: Latency target enforcement; penalizes background cgroups when protected workloads miss target p99 latencies.

## Interface
```bash
# Throttle group to 50MB/s write and 1000 write IOPS
echo "8:0 wbps=52428800 wiops=1000" > /sys/fs/cgroup/prod/io.max
```
