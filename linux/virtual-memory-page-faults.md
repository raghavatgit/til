# Virtual Memory and Page Fault Handling

## Overview
Virtual addresses are translated to physical frames via multi-level page tables (PML4 / 5-level paging on x86_64). When a translation fails in the TLB (Translation Lookaside Buffer) and the page table entry (PTE) is not present or lacks necessary permissions, the MMU triggers a Page Fault (#PF, Interrupt 14).

## Page Fault Resolution Pipeline
1. Hardware saves `CR2` (faulting linear address) and pushes error code onto the kernel stack.
2. Kernel executes `do_page_fault()` and locates the relevant `vm_area_struct` in the process `mm_struct`.
3. Checks access validity (read/write vs VMA permissions).
4. If valid, allocates physical page frame via Buddy Allocator, clears memory if necessary, updates PTE with physical address and present flag, and invalidates TLB.
5. CPU resumes user instruction without application awareness.
