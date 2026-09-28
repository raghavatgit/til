# epoll vs kqueue: Event Notification Mechanics

## Overview
Traditional `select` and `poll` scale at O(n) because the kernel must scan every file descriptor registered upon each invocation. Modern event poll engines (`epoll` on Linux, `kqueue` on BSD/macOS) achieve O(1) event delivery by maintaining in-kernel state.

## epoll Architecture
- `epoll_create1(flags)`: Allocates an anonymous inode and struct eventpoll inside the kernel, containing a red-black tree (for registered FDs) and a doubly linked list (ready list).
- `epoll_ctl(epfd, op, fd, event)`: Adds, modifies, or deletes monitored FDs in the red-black tree in O(log n).
- `epoll_wait(epfd, events, maxevents, timeout)`: Suspends execution until the ready list contains items. When an interrupt fires on a network interface card (NIC), the driver callback executes, placing ready descriptors onto the ready list.

## Edge-Triggered (EPOLLET) vs Level-Triggered
- **Level-Triggered (Default)**: `epoll_wait` returns as long as the underlying buffer has data available to read.
- **Edge-Triggered**: `epoll_wait` notifies user-space only upon state transitions (e.g., from unreadable to readable). The application must drain the socket until `EAGAIN` or `EWOULDBLOCK` is returned.
