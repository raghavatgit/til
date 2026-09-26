# Linux epoll EPOLLEXCLUSIVE & The Thundering Herd Problem

## The Problem
When multiple worker processes listen on the same socket file descriptor (e.g. Nginx or custom multi-process TCP servers) using `epoll_wait()`, an incoming connection event triggers a wakeup on all sleeping processes. Only one process succeeds in calling `accept()`, while remaining processes suffer unnecessary context switches and CPU cache invalidations.

---

## Solution: EPOLLEXCLUSIVE Flag
Introduced in Linux 4.5, `EPOLLEXCLUSIVE` wakes up only a single waiting process from the epoll waitqueue when an event fires:

```c
struct epoll_event ev;
ev.events = EPOLLIN | EPOLLEXCLUSIVE;
ev.data.fd = listen_fd;
epoll_ctl(epfd, EPOLL_CTL_ADD, listen_fd, &ev);
```

### Invariants & Rules
* Only valid for `EPOLL_CTL_ADD`.
* Cannot be combined with `EPOLLONESHOT` or `EPOLLET` (edge-triggered).
* Guarantees starvation-free load distribution across listener worker pools.
