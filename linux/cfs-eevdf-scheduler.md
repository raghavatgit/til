# Linux CPU Scheduling: CFS to EEVDF

## Completely Fair Scheduler (CFS)
- CFS maintained runnable tasks in a time-ordered red-black tree keyed by `vruntime` (virtual runtime).
- Tasks with the smallest `vruntime` are picked next to run.
- `vruntime += delta_exec * (NICE_0_LOAD / task_load)`

## Earliest Eligible Virtual Deadline First (EEVDF)
Introduced in Linux 6.6 to replace CFS:
- CFS prioritized fair throughput over latency, sometimes allowing latency-critical tasks to stall.
- EEVDF introduces two parameters:
  - **Eligibility**: Has the task consumed less than its fair share of CPU?
  - **Virtual Deadline**: When must the task finish its current time slice?
- By scheduling the earliest eligible deadline, EEVDF achieves bounded latency guarantees without compromising proportional share fairness.
