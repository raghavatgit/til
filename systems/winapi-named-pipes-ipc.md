# Windows Named Pipes IPC Across Session Boundaries

*Date: 2026-09-13*  
*Category: Systems Programming / Windows API*

## Overview

When designing background system services or elevated privacy agents (such as StreamShield or daemon managers), communication often spans across session boundaries:
* **Session 0:** Elevated background daemons, system services, or watchdogs.
* **Session 1+:** Interactive user desktop sessions.

Standard Win32 messaging (`PostMessage`, `SendMessage`) is blocked across session boundaries by User Interface Privilege Isolation (UIPI). Windows Named Pipes provide an optimal, high-throughput full-duplex IPC mechanism.

## Pipe Server Creation

Using `CreateNamedPipeW` with `PIPE_ACCESS_DUPLEX` and `PIPE_TYPE_MESSAGE`:

```cpp
#include <windows.h>

HANDLE CreateServicePipe(LPCWSTR pipeName) {
    // DACL configuration: grant Read/Write access to interactive desktop users
    // without granting administrative takeover permissions.
    SECURITY_ATTRIBUTES sa;
    sa.nLength = sizeof(SECURITY_ATTRIBUTES);
    sa.bInheritHandle = FALSE;
    sa.lpSecurityDescriptor = NULL; // Default or custom SDDL string

    HANDLE hPipe = CreateNamedPipeW(
        pipeName,
        PIPE_ACCESS_DUPLEX | FILE_FLAG_OVERLAPPED,
        PIPE_TYPE_MESSAGE | PIPE_READMODE_MESSAGE | PIPE_WAIT,
        PIPE_UNLIMITED_INSTANCES,
        4096,   // Output buffer size
        4096,   // Input buffer size
        5000,   // Default client timeout ms
        &sa
    );

    return hPipe;
}
```

## Atomic Framing and Read Modes

* **Byte Mode (`PIPE_TYPE_BYTE`):** Treats continuous streams like raw TCP sockets; messages require custom delimiter framing.
* **Message Mode (`PIPE_TYPE_MESSAGE`):** Preserves atomic message boundaries. A single `ReadFile` call returns `ERROR_MORE_DATA` if the buffer is too small, allowing zero-copy resizing.

## Key Design Principles

1. **Non-Blocking Overlapped I/O:** Always combine `CreateNamedPipeW` with `FILE_FLAG_OVERLAPPED` and I/O Completion Ports (IOCP) or event handles so a hanging client cannot block the daemon loop.
2. **Strict DACL Permissions:** Never leave pipe security descriptors wide open (`NULL` DACL allows unauthenticated low-integrity processes to spoof messages). Define explicit SDDL strings.
