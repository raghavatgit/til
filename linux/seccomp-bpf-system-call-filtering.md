# seccomp-bpf: System Call Filtering

## Overview
Secure Computing Mode (seccomp) restricts the system calls a process can issue, forming the foundational sandboxing layer in Docker, Podman, and Chromium.

## Architecture
- Uses BPF bytecode programs attached to the process via `prctl(PR_SET_SECCOMP, SECCOMP_MODE_FILTER, &prog)`.
- On every system call, the kernel passes a `seccomp_data` struct containing:
  - `nr`: System call number.
  - `arch`: Target architecture (guards against 32-bit compatibility exploits).
  - `args`: Syscall arguments.

## Return Actions
- `SECCOMP_RET_ALLOW`: Permit execution.
- `SECCOMP_RET_ERRNO`: Return immediate error without executing syscall.
- `SECCOMP_RET_KILL_PROCESS`: Terminate the offending process immediately.
