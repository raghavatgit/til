# Linux cgroup v2 Pressure Stall Information (PSI)

## Beyond Raw Utilization Metrics
High CPU or RAM utilization does not necessarily mean an application is degraded. Pressure Stall Information (PSI) quantifies actual lost wall-clock execution time due to resource starvation across:
1. **CPU:** Threads in runnable state waiting for CPU core execution slots.
2. **Memory:** Execution blocked during page allocation, direct reclaim, or swap-in operations.
3. **I/O:** Execution stalled waiting for block device read/write completion.

---

## PSI Metric Hierarchy
* `some`: Percentage of time in which at least one task stalled on the resource.
* `full`: Percentage of time in which all non-idle tasks stalled simultaneously (complete system deadlock).

Monitored via `/proc/pressure/{cpu,memory,io}` or cgroup-specific `/sys/fs/cgroup/<group>/memory.pressure`.
