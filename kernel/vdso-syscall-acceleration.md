# Virtual Dynamically Linked Shared Object (vDSO)

## Problem Statement
Standard syscalls (`syscall` / `sysenter` on x86) trigger CPU privilege level transitions from Ring 3 (userspace) to Ring 0 (kernel), flushing pipelines and incurring ~50-100ns latency.

## Architecture
The Linux kernel maps a read-only shared ELF library (`[vdso]`) into every process virtual address space. Time-sensitive calls like `clock_gettime()`, `gettimeofday()`, and `time()` read kernel-maintained timestamps directly from memory or CPU timestamp counters (`RDTSC`) without switching rings.
