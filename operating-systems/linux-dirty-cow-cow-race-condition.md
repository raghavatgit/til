# Linux Dirty COW (CVE-2016-5195) Race Condition

## Vulnerability Mechanics
In the Linux kernel's Memory Management subsystem, Copy-On-Write (COW) allows multiple processes to share read-only memory pages. When a process attempts a write, the kernel allocates a new physical copy, marks it writable, and points the page table entry (PTE) to the new page.

In Dirty COW, two threads race within `madvise(MADV_DONTNEED)` and `write()` on `/proc/self/mem`:
1. **Thread 1:** Calls `write()` on `/proc/self/mem` targeting a read-only memory mapping. The kernel fault handler allocates a private copy and sets the dirty flag.
2. **Thread 2:** Rapidly calls `madvise(..., MADV_DONTNEED)`, discarding the private copy and forcing the kernel to re-fault.
3. **The Race:** If `MADV_DONTNEED` runs between the `follow_page_mask()` read check and the actual write operation, the kernel mistakenly writes the modified data directly to the original underlying read-only physical page (such as `/bin/su` or read-only page cache).
