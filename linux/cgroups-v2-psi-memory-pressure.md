# cgroups v2 PSI: Real-Time Memory Pressure Triggers

## Traditional Load Averages vs PSI
Load averages reflect queue length of runnable tasks but cannot distinguish whether tasks are waiting for CPU, disk I/O, or paging memory. Pressure Stall Information (PSI) measures wall-clock time loss.

## File Triggers
Applications can register for low-latency kernel events by writing a threshold specification to `/sys/fs/cgroup/memory.pressure`:
`some 150000 1000000`
- Fires when memory stall time exceeds 150ms over a 1-second rolling tracking window.
- The monitoring daemon polls the file descriptor with `epoll_wait()`, enabling proactive cache shedding before the Linux OOM killer is triggered.
