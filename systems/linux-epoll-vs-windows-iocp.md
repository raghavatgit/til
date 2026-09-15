# Asynchronous I/O Models: Linux epoll vs Windows IOCP

*Date: 2026-09-15*  
*Category: Systems Programming / High-Concurrency Networking*

## Overview

High-concurrency network servers (such as Nginx, Node.js libuv, or Rust Tokio) handle hundreds of thousands of concurrent connections using operating system event multiplexers. However, Linux and Windows implement fundamentally distinct design philosophies:

1. **Linux `epoll` (Readiness Model):** The kernel notifies user space when a socket is *ready* to read or write. User space then initiates `read()` or `write()` syscalls to copy data.
2. **Windows `IOCP` (Completion Model):** User space initiates asynchronous operations (`WSARecv`, `WSASend`) upfront. The Windows kernel performs the I/O transfer and notifies a thread pool when the operation is *complete*.

## Architectural Comparison

| Dimension | Linux epoll | Windows IOCP |
| :--- | :--- | :--- |
| **Notification Type** | Readiness (`EPOLLIN`, `EPOLLOUT`) | Completion (`GetQueuedCompletionStatus`) |
| **Buffer Management** | User allocates buffer on readiness | Buffer locked in kernel memory during I/O |
| **Thread Model** | Event loop pulls ready events | Kernel manages thread pool concurrency limit |
| **Zero-Copy Potential** | Needs `vmsplice` or `io_uring` | Native overlapped zero-copy buffers |

## Code Architecture Pattern (IOCP)

```cpp
#include <windows.h>

// Associate socket with I/O Completion Port
HANDLE CreateServerIOCP(SOCKET sock, HANDLE hIOCP, ULONG_PTR completionKey) {
    return CreateIoCompletionPort(
        (HANDLE)sock,
        hIOCP,
        completionKey,
        0 // Concurrency: match system CPU core count
    );
}

// Worker thread consumption loop
void WorkerThread(HANDLE hIOCP) {
    DWORD bytesTransferred = 0;
    ULONG_PTR completionKey = 0;
    LPOVERLAPPED pOverlapped = NULL;

    while (TRUE) {
        BOOL ok = GetQueuedCompletionStatus(
            hIOCP,
            &bytesTransferred,
            &completionKey,
            &pOverlapped,
            INFINITE
        );

        if (ok && pOverlapped) {
            // Buffer already filled by kernel; zero-copy processing ready
            ProcessCompletedPacket(pOverlapped, bytesTransferred);
        }
    }
}
```

## Impact on Runtimes
Libraries like `libuv` (Node.js) and `mio` (Rust) abstract both models into a uniform polling interface, hiding readiness vs completion differences from application developers.
