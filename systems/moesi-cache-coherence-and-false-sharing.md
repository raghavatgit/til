# MOESI Cache Coherence Protocol & False Sharing Mitigations

## Transitions in the MOESI Protocol
The MOESI protocol extends standard MESI by adding the **Owned (O)** state to optimize inter-core data transfers on shared bus architectures (e.g., AMD Infinity Fabric and modern multi-socket interconnects).

---

## The 5 MOESI States

| State | Dirty? | Shared? | Memory Synchronized? | Direct Inter-Core Transfer? |
| :--- | :--- | :--- | :--- | :--- |
| **Modified (M)** | Yes | No (Exclusive) | No | Yes |
| **Owned (O)** | Yes | Yes | No (Inter-core shared dirty) | Yes (Serves read requests) |
| **Exclusive (E)** | No | No (Exclusive) | Yes | Yes |
| **Shared (S)** | No | Yes | Yes | Read-only |
| **Invalid (I)** | N/A | N/A | N/A | Must fetch from bus |

The **Owned** state permits a dirty cache line to be shared among multiple core L1/L2 caches without writing back to main RAM first.

---

## False Sharing Mechanics
Hardware caches operate at the granularity of **Cache Lines** (typically 64 bytes).
- When Thread 1 writes to variable `A` and Thread 2 writes to variable `B`, if both variables reside in the same 64-byte physical line:
  - Each write invalidates the peer core's entire cache line.
  - The line constantly bounces between cores over the interconnect bus, destroying multi-threaded throughput.

### Hardware Mitigation
Pad concurrent thread-local structures to cache line boundaries:
```cpp
struct alignas(64) ThreadWorkerStats {
    uint64_t operations_completed;
    // 56 bytes padding to isolate next thread's cache line
};
```
