# Linux Socket SO_REUSEPORT & Kernel Load Balancing

## The Listener Bottleneck
Historically, a single listening socket accepted incoming connections. When multiple threads called `accept()`, lock contention on the listen queue degraded multicore throughput.

---

## SO_REUSEPORT Mechanics
Introduced in Linux 3.9, `SO_REUSEPORT` allows multiple independent socket descriptors to bind to the exact same IP address and port:
* Each thread or process owns an independent listen queue.
* The Linux kernel hashes the incoming connection 4-tuple: `hash(src_ip, src_port, dst_ip, dst_port) % num_sockets`.
* Routes new TCP connections directly to one of the listen queues without inter-thread locking.
