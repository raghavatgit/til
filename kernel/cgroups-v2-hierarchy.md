# Control Groups v2 (cgroups v2) Unified Hierarchy

## Architecture
Unlike cgroups v1 where each resource subsystem (memory, cpu, blkio, pids) maintained independent hierarchy trees, cgroups v2 enforces a single unified tree.

## Core Invariants
- No Internal Processes: A cgroup node can either host sub-cgroups or host processes, but never both simultaneously (except the root cgroup).
- Integrated Memory + Swap Accounting: Replaces isolated v1 accounting with unified `memory.high` throttling and `memory.max` out-of-memory killing.
