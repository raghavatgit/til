# Copy-on-Write (COW) in Linux fork()

## fork() Mechanics
When a process calls `fork()`, the kernel duplicates the parent's `task_struct`, file descriptor table, and page tables, but does NOT duplicate physical memory pages.

## Page Protection Modification
1. Parent and child page tables are pointed to the exact same physical frames.
2. The kernel clears the Write bit and sets the Read-Only bit in all shared PTEs.
3. Reference counts on the underlying physical page structs are incremented.

## On Write Trigger
When either process writes to a shared page:
1. CPU raises page fault Interrupt 14 (write violation on read-only page).
2. Kernel checks if the page is COW: if reference count > 1, it allocates a new physical frame, copies the 4 KB content, assigns the new page to the faulting process, marks it writable, and invalidates TLB.
