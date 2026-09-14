# Windows Virtual Memory Management: Reservation vs Commitment

*Date: 2026-09-14*  
*Category: Systems Programming / Windows Internals*

## Overview

The Windows NT Virtual Memory Manager provides a two-phase allocation lifecycle (`VirtualAlloc`):
1. **Address Space Reservation (`MEM_RESERVE`):** Claims a contiguous range of the process's 128 TB (on x64) virtual address space without allocating physical RAM or backing pagefile space.
2. **Page Commitment (`MEM_COMMIT`):** Charges memory against the system commit limit, establishing page table entries backed by physical RAM or the paging file on first access (demand paging).

## Two-Phase Allocation Pattern

```cpp
#include <windows.h>
#include <iostream>

void DemonstrateTwoPhaseAllocation() {
    // Reserve 1 GB of virtual address space (costs 0 physical RAM)
    SIZE_T reserveSize = 1024 * 1024 * 1024; // 1 GB
    LPVOID pBase = VirtualAlloc(
        NULL,
        reserveSize,
        MEM_RESERVE,
        PAGE_NOACCESS
    );

    if (!pBase) return;

    // Dynamically commit only the first 64 KB as workload grows
    SIZE_T commitSize = 64 * 1024; // 64 KB
    LPVOID pCommitted = VirtualAlloc(
        pBase,
        commitSize,
        MEM_COMMIT,
        PAGE_READWRITE
    );

    if (pCommitted) {
        // Safe to write: triggers a minor page fault to map a physical page frame
        int* pInt = (int*)pCommitted;
        *pInt = 42;
    }

    // Release all reserved memory
    VirtualFree(pBase, 0, MEM_RELEASE);
}
```

## Key Principles
* **Sparse Arrays and Arenas:** High-performance allocators (e.g. jemalloc, mimalloc, game engines) reserve large multi-gigabyte address ranges upfront to guarantee contiguous indexing, committing 4 KB pages incrementally on demand.
* **Working Set Trimming:** The operating system periodically trims inactive committed pages from a process's Working Set (`KPROCESS`) into the Standby page list, reclaiming physical RAM without data loss.
