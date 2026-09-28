# cgroups v2: Unified Resource Control

## cgroups v1 vs cgroups v2
In cgroups v1, every resource subsystem (cpu, memory, blkio, pids) maintained an independent tree hierarchy, making cross-controller coordination (such as buffered writeback throttling) nearly impossible.
cgroups v2 introduces a single unified hierarchy rooted at `/sys/fs/cgroup`.

## Controllers
- `memory`: Strict hierarchical limits (`memory.max`, `memory.high` for proactive throttling).
- `cpu`: Quota and weight scheduling (`cpu.max = $QUOTA $PERIOD`).
- `io`: Block I/O bandwidth and IOPS limits (`io.max`).
- `pids`: Fork bomb prevention (`pids.max`).

## Pressure Stall Information (PSI)
cgroups v2 integrates PSI metrics (`memory.pressure`, `cpu.pressure`, `io.pressure`) exposing real-time visibility into the percentage of wall-clock time tasks spend stalled waiting on resources.
