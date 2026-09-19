# Linux Kernel: Completely Fair Scheduler (CFS) and vruntime

## The Concept of Virtual Runtime (vruntime)
The Completely Fair Scheduler (CFS) models an ideal multi-tasking CPU where $N$ processes run simultaneously at $1/N$ speed:
- `vruntime` measures the execution time spent by a task, scaled inversely by its nice level (priority weight).
- Nice value 0 has weight 1024. A higher priority task (negative nice) accumulates `vruntime` slower, granting it more physical CPU execution time.

## Red-Black Tree Runqueue Architecture
CFS organizes runnable tasks in a time-ordered red-black tree (`cfs_rq`):
- Leftmost node has the smallest `vruntime` (the task most deprived of CPU time).
- `schedule()` picks the leftmost node in $O(1)$ time (cached pointer `rb_leftmost`).
- When a task finishes its timeslice, its `vruntime` increments, and it is reinserted into the RB-tree in $O(\log N)$ time.

## Latency Target and Granularity
- `sysctl_sched_latency`: Target period in which all runnable tasks should execute at least once.
- `sysctl_sched_min_granularity`: Minimum timeslice allocated to a task to prevent thrashing from excessive context switches.
