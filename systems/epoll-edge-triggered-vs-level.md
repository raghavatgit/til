# Epoll: Edge-Triggered vs Level-Triggered Notifications

## Level-Triggered (Default)
`epoll_wait()` returns as long as the file descriptor buffer contains unread data. Safer, but produces redundant wakeups.

## Edge-Triggered (`EPOLLET`)
`epoll_wait()` notifies userspace ONLY when the readiness state transitions (e.g. from empty to non-empty).
Invariant: Readers MUST loop `read()` until it returns `EAGAIN` or `EWOULDBLOCK`, otherwise remaining bytes will never trigger a notification.
