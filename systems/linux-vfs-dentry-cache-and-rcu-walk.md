# Linux VFS Dentry Cache & RCU-Walk Mechanics

## Problem Space & Bottleneck
Resolving file paths like `/usr/lib/x86_64-linux-gnu/libc.so.6` requires traversing multiple path components across filesystem boundaries. A traditional naive implementation acquires reference counters (`dget`) and locks on each directory entry (`dentry`), inducing massive bus contention and atomic cache-line bouncing on high-core servers.

---

## Architectural Mechanism: RCU-Walk vs Ref-Walk
The Linux kernel decomposes path resolution (`fs/namei.c`) into two phases:

1. **RCU-Walk (Optimistic Fast-Path)**:
   - Traversal starts under an RCU read-side critical section (`rcu_read_lock`).
   - No reference counts (`d_count`) are incremented on intermediate dentries.
   - Sequential lock sequence numbers (`seqcount`) check whether a directory rename or deletion invalidated the dentry while being traversed.
   - If memory ordering or concurrent mutation is detected (or if filesystem requires blocking operations like uncached disk read), the traversal seamlessly aborts and falls back to **Ref-Walk**.

2. **Ref-Walk (Pessimistic Slow-Path)**:
   - Takes explicit dentry reference counts (`dget`).
   - Handles blocking disk I/O, symlink evaluations spanning remote filesystems, and complex permission checks.

---

## Memory Layout of `struct dentry`
```c
struct dentry {
    unsigned int d_flags;       /* lookup flags */
    seqcount_spinlock_t d_seq;  /* per-dentry seqlock */
    struct hlist_bl_node d_hash;/* lookup hash list */
    struct dentry *d_parent;    /* parent directory */
    struct qstr d_name;         /* name string and hash */
    struct inode *d_inode;      /* associated VFS inode */
    struct list_head d_child;   /* child dentries */
    struct list_head d_subdirs; /* subdirectories list */
};
```

---

## Performance Implications
- Lockless RCU-walk achieves near-linear multicore scaling during high-throughput static file serving and container runtime binary loading.
- Avoids cacheline invalidations on shared parent directories like `/`, `/usr`, and `/var`.
