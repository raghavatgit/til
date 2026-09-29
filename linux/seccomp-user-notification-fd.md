# seccomp User Notification: Unprivileged Syscall Emulation

## Architecture
`SECCOMP_RET_USER_NOTIF` allows an unprivileged process inside a container to yield handling of sensitive syscalls (such as `mount` or `mknod`) to an external privileged supervisor process via a dedicated file descriptor.

## Workflow
1. Sandboxed process issues syscall.
2. Kernel pauses calling thread and sends notification struct across notification fd.
3. Supervisor inspects syscall arguments, executes necessary host operations, and injects return value or error code back via `ioctl(SECCOMP_IOCTL_NOTIF_SEND)`.
4. Sandboxed thread resumes without requiring root capabilities.
