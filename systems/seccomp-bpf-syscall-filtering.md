# Linux Security: Seccomp-BPF System Call Sandboxing

## Operating Principle
Secure Computing Mode with BPF (Seccomp-BPF) permits an unprivileged process to attach a custom BPF filter program to its system call dispatcher via `prctl(PR_SET_SECCOMP, SECCOMP_MODE_FILTER, &prog)`.

## Filter Actions
- `SECCOMP_RET_ALLOW`: Permit syscall execution normally.
- `SECCOMP_RET_KILL_PROCESS`: Instantly terminate process (for high-severity violations).
- `SECCOMP_RET_ERRNO`: Block syscall and return a simulated error code (e.g. `EPERM`).
- `SECCOMP_RET_TRACE`: Notify aptrace debugger for dynamic inspection.
- `SECCOMP_RET_LOG`: Allow syscall but write an audit log message to kernel ring buffer.

## Standard Container Sandboxing
Docker and Podman apply a baseline seccomp profile that disables ~44 high-risk syscalls out of 400+ (such as `sys_chroot`, `keyctl`, `bpf`, `kexec_load`).
