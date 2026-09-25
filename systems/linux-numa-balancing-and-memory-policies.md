# Linux NUMA Topology & Memory Allocation Policies

## Non-Uniform Memory Access (NUMA) Dynamics
In modern multi-socket server architectures, memory access latency depends on the physical distance between the CPU core and the memory controller:
- **Local Access**: ~50-80 ns.
- **Remote Access (UPI/QPI interconnect hop)**: ~120-180 ns.

---

## Memory Allocation Policies (`mbind` & `numactl`)

1. **`MPOL_DEFAULT`**:
   - Allocates memory on the local NUMA node of the executing thread.
   - If local memory is exhausted, falls back to neighboring nodes.

2. **`MPOL_BIND`**:
   - Strictly confines allocations to a designated set of NUMA nodes.
   - Throws `ENOMEM` if the allowed node capacity is reached.

3. **`MPOL_INTERLEAVE`**:
   - Round-robin page allocation across specified nodes.
   - Optimal for bandwidth-bound, large dataset workloads where no single node has sufficient memory bus bandwidth.

---

## Kernel Automatic NUMA Balancing
- Periodically marks pages as non-present (`PROT_NONE`).
- On access, triggers a minor page fault (`do_numa_page`).
- Kernel records the requesting CPU ID and checks access locality.
- If repeated cross-node memory accesses occur, kernel migrates the physical page to the accessing thread's local node (`task_numa_placement`).
