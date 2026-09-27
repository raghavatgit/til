# Linux cgroup v2 Memory Thresholds: memory.high vs memory.max

## The Problem with Binary OOM Kills
In cgroup v1, exceeding `memory.limit_in_bytes` triggered an immediate Out-Of-Memory (OOM) killer invocation, abruptly terminating critical workloads without warning.

---

## cgroups v2 Hierarchical Throttling
cgroup v2 introduces smooth, proactive memory governance:

1. **`memory.high` (Throttle & Reclaim Threshold):**
   - When exceeded, processes in the cgroup are throttled and forced into synchronous kernel reclaim.
   - Userland daemons (e.g. systemd-oomd) receive notifications to scale down or Shed load before hard limits.
2. **`memory.max` (Hard Upper Bound):**
   - Reaching this triggers aggressive page cache evacuation. If reclaim fails to free memory below `memory.max`, the OOM killer executes.
3. **`memory.peak`:**
   - Read-only watermark recording the absolute maximum memory usage reached by the cgroup since creation.
