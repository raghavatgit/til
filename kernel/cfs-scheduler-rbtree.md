# Completely Fair Scheduler (CFS) Architecture

## Virtual Runtime (`vruntime`)
CFS models an "ideal multi-tasking CPU". Each runnable thread tracks `vruntime`:
vruntime += (delta_exec_time * NICE_0_LOAD) / se->load.weight

## Red-Black Tree Organization
Runnable tasks are indexed in a self-balancing Red-Black tree (`cfs_rq->tasks_timeline`) sorted by `vruntime`. The scheduler always picks the leftmost node (`rb_leftmost`), guaranteeing O(1) selection of the task with the least executed virtual time.
