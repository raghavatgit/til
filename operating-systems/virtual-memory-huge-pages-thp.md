# Virtual Memory: Transparent Huge Pages (THP) vs Latency

## Background: The Cost of TLB Misses
Modern CPU architectures translate virtual memory addresses to physical addresses using multi-level page tables. The Translation Lookaside Buffer (TLB) caches recent translations.
- Standard page size on x86-64 is 4 KiB.
- A 64 GiB heap requires 16,777,216 pages, overwhelming L1/L2 TLB capacities (typically 64-1536 entries).
- TLB misses trigger costly hardware page table walks (4 memory accesses per miss).

## Huge Pages (2 MiB and 1 GiB)
Using 2 MiB pages:
- TLB coverage increases by a factor of 512 per TLB entry.
- Page table hierarchy drops from 4 levels to 3 levels.
- Drastically reduces TLB miss rates for contiguous sequential access patterns.

## The Problem with Transparent Huge Pages (THP)
Linux kernel introduced Transparent Huge Pages (THP) to automatically group contiguous 4 KiB pages into 2 MiB pages without application code changes. However, in high-throughput database workloads (Redis, MongoDB, RocksDB, PostgreSQL), THP frequently causes severe tail latency spikes:
1. **Direct Memory Compaction**: When contiguous 2 MiB physical blocks are scarce, the kernel initiates synchronous memory compaction, locking memory regions while relocating physical pages.
2. **Page Fault Latency**: Zeroing or allocating a 2 MiB page takes significantly longer than a 4 KiB page.
3. **Memory Bloat**: Sparse write workloads in a 2 MiB block allocate the full 2 MiB in physical RAM, triggering premature out-of-memory (OOM) conditions.

## Production Recommendation
For databases and low-latency engines:
```bash
# Disable runtime THP compaction
echo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled
echo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag

# Provision explicit Hugetlbfs pages instead
echo 2048 | sudo tee /proc/sys/vm/nr_hugepages
```
Explicit hugepages avoid runtime synchronous compaction locks while delivering deterministic TLB reach.
