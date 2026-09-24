# CPU Cache Coherence: MESI Protocol and False Sharing

## MESI States
- **Modified (M)**: Line is present only in current cache and is dirty (differs from main memory).
- **Exclusive (E)**: Line is present only in current cache and is clean.
- **Shared (S)**: Line may be stored in other caches and is clean.
- **Invalid (I)**: Line does not contain valid data.

## False Sharing
False sharing occurs when independent threads on different cores modify distinct variables that reside within the same 64-byte cache line.
Even though variables are conceptually disjoint, writing to one invalidates the entire cache line across all other cores via the MESI bus protocol, inducing massive bus contention.

## Mitigation in C++
```cpp
struct alignas(64) WorkerState {
    uint64_t counter;
};
```
