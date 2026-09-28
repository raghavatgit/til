# Consistent Hashing with Virtual Nodes

## The Resharding Problem
Standard modulus hashing `hash(key) mod N` invalidates almost all data ($N / (N+1)$ keys) when nodes are added or removed.

## Ring Architecture
1. Hash keys and node identifiers onto a 32-bit or 128-bit ring space ($[0, 2^{128}-1]$).
2. A key is assigned to the first node encountered walking clockwise along the ring.

## Virtual Nodes (Vnodes)
- Single physical machines are assigned multiple virtual positions (e.g., 256 vnodes per physical node).
- Benefits:
  - Eliminates non-uniform hash skew.
  - On node failure, load is distributed evenly across all remaining nodes rather than overwhelming the immediate neighbor.
