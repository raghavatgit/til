# Linux Landlock: Unprivileged Sandboxing (Linux 5.13+)

## Sandboxing Without Root Privileges
Traditional Linux isolation mechanisms (chroot, seccomp, namespaces) often require `CAP_SYS_ADMIN` root privileges. Landlock allows any unprivileged application to sandbox itself safely.

---

## Filesystem Ruleset Enforcement
An application defines an access ruleset restricting file operations (e.g. `LANDLOCK_ACCESS_FS_READ_FILE`) to specific directory trees, and invokes `landlock_restrict_self()`. Once locked, child processes inherit the restrictions and cannot escape.
