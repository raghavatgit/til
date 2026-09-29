# Landlock LSM Unprivileged Application Sandboxing

Landlock is a Linux Security Module (LSM) enabling non-root processes to create restrictive filesystem and network access sandboxes.

## Key Properties
- Inherited across `fork()` and preserved across `execve()` when `PR_SET_NO_NEW_PRIVS` is set.
- Composable rulesets: Sandboxes can only restrict further, never widen permissions.

## Syscall Sequence
1. `landlock_create_ruleset()`: Defines desired access rights mask (`LANDLOCK_ACCESS_FS_READ_FILE`, `LANDLOCK_ACCESS_NET_BIND_TCP`).
2. `landlock_add_rule()`: Binds path file descriptors to permitted access rights.
3. `landlock_restrict_self()`: Enforces the sandbox on current thread and descendants.
