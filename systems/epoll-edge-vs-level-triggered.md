# Epoll: Edge-Triggered vs Level-Triggered Semantics

## Behavior Comparison
- **Level-Triggered (Default)**: `epoll_wait` repeatedly notifies as long as the file descriptor buffer contains unread bytes. Safe, simple, but incurs redundant wakeups if not drained in one go.
- **Edge-Triggered (`EPOLLET`)**: `epoll_wait` notifies only when a state transition occurs (e.g. from no data available to data available).

## Requirements for Edge-Triggered (`EPOLLET`)
1. File descriptor must be set to non-blocking (`O_NONBLOCK`).
2. Must loop `read()` or `write()` until `EAGAIN` or `EWOULDBLOCK`.
3. Beware starvation: looping until `EAGAIN` on a noisy socket can starve other sockets in the epoll event set. A loop budget (e.g. max 64KB per turn) is essential.
