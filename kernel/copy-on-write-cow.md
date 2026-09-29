# Copy-On-Write (COW) in Linux Virtual Memory

## Mechanism during fork()
When `fork()` is called:
1. The kernel clones the parent process page directory structure without copying physical page frames.
2. All shared pages are marked read-only (`PAGE_RO`) in both parent and child page tables.
3. Page reference counters in `struct page` are incremented.

## Page Fault Resolution
When either parent or child writes to a shared page:
1. The CPU generates a Page Fault (#PF, exception 14) due to write permission violation.
2. The kernel page fault handler inspects VMA permissions and discovers the COW flag.
3. A new physical page frame is allocated, data is copied, the writing process table is updated to read-write, and the old page reference count is decremented.
