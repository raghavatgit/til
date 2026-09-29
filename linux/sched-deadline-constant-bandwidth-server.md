# SCHED_DEADLINE and Constant Bandwidth Server (CBS)

## Overview
`SCHED_DEADLINE` implements Earliest Deadline First (EDF) with Constant Bandwidth Server (CBS) to provide hard real-time scheduling guarantees on Linux.

## Scheduling Tuple: (Runtime, Deadline, Period)
- Every task specifies $Q$ (runtime budget), $D$ (deadline), and $P$ (period) where $Q \le D \le P$.
- Guarantees $Q$ CPU time every $P$ period.
- **CBS Enforcement**: If a task attempts to execute longer than its allocated budget $Q$, CBS throttles the task until the next period, protecting other real-time processes from denial of service.
