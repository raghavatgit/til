# Operating Systems: Linux cgroup v2 Memory Controller and OOM Killer

## Hierarchical Memory Limits
Under Linux `cgroup v2`, memory boundaries isolate containerized workloads:
- `memory.min`: Hard memory protection. Kernel never reclaims memory below this boundary under global pressure.
- `memory.low`: Best-effort protection. Memory reclaimed only when unallocated RAM is scarce.
- `memory.high`: Throttling boundary. Exceeding this forces processes into synchronous page reclamation and delays allocations.
- `memory.max`: Hard limit. Exceeding this boundary triggers memory allocation failure or invoking the Out-of-Memory (OOM) killer.

## The OOM Killer Selection Heuristic
When physical RAM and swap are exhausted, the kernel invokes `out_of_memory()`:
- Computes `badness` score for each task based on physical RAM consumption (`total_vm`).
- Scales score by `oom_score_adj` (range: -1000 to +1000):
  - `-1000`: Disables OOM killing entirely (used for SSH daemons or critical hypervisors).
  - `+1000`: Guarantees task is selected first for termination (used for ephemeral batch workers).
