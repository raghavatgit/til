# Linux task_struct & PID Allocation Internals

## The Process Descriptor
Every thread and process in Linux is represented by `struct task_struct` in `include/linux/sched.h`. Key structures include:
* `mm_struct`: Virtual memory maps, page directories, code/heap/stack segments.
* `files_struct`: File descriptor tables and reference counts.
* `signal_struct`: Pending signals and signal action handlers.
* `thread_info`: Low-level CPU architectural registers and flags.

---

## PID Bitmaps & Namespaces
PIDs are managed via hierarchical `struct pid_namespace`:
1. The kernel maintains a PID bitmap array (`pidmap`) tracking allocated integer identifiers.
2. When `fork()` is called, `alloc_pid()` allocates a distinct PID struct having multiple numerical IDs, one for each nested namespace level from the leaf namespace up to the root init namespace.
