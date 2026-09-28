# Redis Memory Eviction: Approximated LRU and LFU

## Eviction Trigger
When memory usage exceeds `maxmemory`, Redis executes an eviction policy (`allkeys-lru`, `volatile-lru`, `allkeys-lfu`).

## Approximated LRU
Exact LRU requires maintaining a doubly-linked list of all keys, incurring prohibitive memory overhead ($16+$ bytes per key).
Redis uses sampling:
1. Each key object header stores 24 bits of idle time (`lru` clock).
2. On eviction, Redis samples $N$ random keys (default 5) and evicts the one with largest idle time.
3. Increasing sample size to 10 approaches true LRU precision within statistical margin.

## LFU (Least Frequently Used)
Compresses access frequency into an 8-bit logarithmic counter with exponential decay over time.
