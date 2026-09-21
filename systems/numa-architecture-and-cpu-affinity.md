# Systems: NUMA Architecture, Memory Domains, and CPU Pinning

## Uniform vs Non-Uniform Memory Access
In multi-socket servers:
- **UMA (Uniform Memory Access)**: All CPUs share a single memory bus. High memory bus contention beyond 8-16 cores.
- **NUMA (Non-Uniform Memory Access)**: Each CPU socket possesses dedicated memory channels (local NUMA node). Accessing remote node memory traverses inter-socket interconnects (Intel UPI, AMD Infinity Fabric).
- **Latency Disparity**: Local node RAM access latency is $\approx 60-80\text{ns}$; remote node access latency is $\approx 120-160\text{ns}$ ($2\times$ latency overhead).

## Operating System NUMA Policies
Linux memory allocation policies (`numactl`, `set_mempolicy`):
- `MPOL_BIND`: Strictly allocates memory from specified NUMA nodes. Fails if local node exhausts RAM.
- `MPOL_PREFERRED`: Favors local node; spills over to remote nodes under memory pressure.
- `MPOL_INTERLEAVE`: Round-robins page allocations across nodes (useful for memory-bound shared caches).

## Thread CPU Pinning (Affinity)
Binding critical threads to dedicated CPU cores on the local NUMA node:
```c
#define _GNU_SOURCE
#include <sched.h>

cpu_set_t cpuset;
CPU_ZERO(&cpuset);
CPU_SET(target_core_id, &cpuset);
pthread_setaffinity_np(thread_handle, sizeof(cpu_set_t), &cpuset);
```
Prevents operating system scheduler migrations from thrashing L1/L2 caches across NUMA domains.
