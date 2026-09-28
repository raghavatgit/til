# VFS Dentry and Inode Caching Lifecycle

## Virtual Filesystem Abstraction
Linux VFS decouples filesystem client calls (`open`, `stat`, `unlink`) from specific storage implementations (ext4, XFS, btrfs, NFS).

## Core VFS Objects
1. `inode`: Represents the file metadata (ownership, mode, timestamps, block pointers).
2. `dentry` (Directory Entry): Represents path components and maps names to inodes.
3. `file`: Represents an open file instance (tracks file offset, flags, and open mode).
4. `super_block`: Represents an entire mounted filesystem instance.

## Dcache (Dentry Cache)
Resolving `/var/log/syslog` requires hierarchical component resolution. The dcache retains frequently accessed dentries in an RCU-protected hash table, converting path lookups into high-speed memory traversals without disk I/O.
