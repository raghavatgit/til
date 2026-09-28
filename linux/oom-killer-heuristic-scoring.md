# Linux OOM Killer: Heuristic Scoring and Tuning

## Invocation
When the Buddy Allocator exhausts all memory zones and page reclaim / compaction fails to satisfy an allocation, the kernel triggers `out_of_memory()`.

## oom_badness Calculation
The kernel scans all processes and computes a score (`oom_score` between 0 and 1000):
`points = process_resident_pages + process_swap_pages`
- Favors killing processes consuming the most memory with minimal structural disruption.
- Excludes `kthreads` and `init` (PID 1).

## Tuning
- `/proc/[pid]/oom_score_adj`: Range [-1000, 1000]. Setting -1000 disables OOM killing for the process (standard for databases and critical supervisors).
- `vm.panic_on_oom`: Forces kernel panic instead of killing processes.
