# HugePages and Transparent HugePages (THP) Tuning

## Problem
Standard page size on x86_64 is 4 KB. For a database with 64 GB of memory, managing memory with 4 KB pages requires 16,777,216 page table entries. The CPU TLB holds only a few thousand entries, resulting in severe TLB thrashing and up to 20% CPU overhead spent purely on page table walks.

## HugePages (2 MB and 1 GB)
- 2 MB pages reduce the number of required PTEs by a factor of 512.
- 1 GB pages reduce it by 262,144.

## Explicit HugePages vs THP
- **Transparent HugePages (THP)**: Kernel daemon `khugepaged` opportunistically merges 4 KB pages into 2 MB pages. Can induce latency spikes due to memory defragmentation locks.
- **Static HugePages**: Pre-allocated at boot via `sysctl vm.nr_hugepages=N` or `hugepagesz=1G`. Ideal for predictable low-latency workloads (PostgreSQL, Redis, ScyllaDB).
