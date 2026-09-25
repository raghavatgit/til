# Systems: Linux cgroups v2 Architecture and Unified Resource Isolation

## Unified Hierarchy vs Legacy cgroups v1
Linux cgroups v1 suffered from multiple orthogonal controller hierarchies (cpu, memory, blkio in separate trees), making coordinated multi-resource accounting and writeback attribution impossible.
cgroups v2 enforces a strict single-hierarchy model mounted at `/sys/fs/cgroup`.

## Core Controllers
1. **`memory`**:
   - `memory.max`: Hard limit triggering OOM killer when exceeded.
   - `memory.high`: Throttle boundary inducing allocation delays and aggressive reclaim without killing processes.
   - `memory.current`: Live byte usage including page cache and slab.
2. **`cpu`**:
   - `cpu.weight`: Proportional share scheduling (CFS).
   - `cpu.max`: Bandwidth quota `[quota, period]` (e.g. `200000 100000` = 2 cores).
3. **`io`**:
   - Bandwidth and IOPS limits per block device major:minor number.
4. **`pids`**:
   - `pids.max`: Hard cap on thread/process count to prevent fork bombs.
