# Linux VFS: Path Lookup Scalability with SeqLocks and RCU (dcache)

## The Dentry Cache (dcache) Scalability Bottleneck
Path resolution (e.g. `/usr/local/bin/app`) traverses directory entry structures (`dentry`):
- Legacy kernels protected dentry nodes with read-write spinlocks.
- On 64+ core machines, concurrent `stat(2)` calls caused severe lock cache-line bouncing.

## RCU-Walk vs Ref-Walk
Modern Linux implements lockless path lookup:
1. **RCU-Walk Mode**:
   - Traversal acquires zero locks and performs zero atomic reference count modifications on dentries.
   - Protected by `rcu_read_lock()` and sequence locks (`seqcount_t` in `struct dentry`).
2. **SeqLock Validation**:
   - Reader checks dentry sequence counter before and after reading name/inode pointers.
   - If sequence changed (concurrent rename or deletion), traversal cleanly aborts RCU-walk and falls back to traditional ref-walk.
- Achieves linear multi-core scaling for filesystem metadata reads.
