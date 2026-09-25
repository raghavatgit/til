# Linux Kernel: The 8 Namespaces Powering Modern Containers

## Namespace Types
Namespaces partition global system resources into isolated virtual views:
1. **`PID`**: Process IDs virtualized (isolated process sees itself as PID 1).
2. **`NET`**: Independent network stack (interfaces, routing tables, iptables rules, sockets).
3. **`MNT`**: Isolated filesystem mount points without affecting host mounts.
4. **`IPC`**: System V IPC message queues, semaphores, and POSIX shared memory.
5. **`UTS`**: Hostname and NIS domain name isolation.
6. **`USER`**: Maps user/group IDs (e.g. UID 0 inside namespace maps to unprivileged UID 10001 on host).
7. **`CGROUP`**: Virtualized view of `/sys/fs/cgroup` relative to container root.
8. **`TIME`**: Independent monotonic and boot clock offsets (Linux 5.6+).

## Syscall Primitives
- `clone(..., CLONE_NEWPID | CLONE_NEWNET | ...)`: Creates a new child process in new namespaces.
- `unshare(flags)`: Disassociates current process from parent namespaces.
- `setns(fd, nstype)`: Attaches current thread to an existing namespace.
