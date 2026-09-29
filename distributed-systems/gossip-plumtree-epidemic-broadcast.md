# Plumtree: Push-Lazy-Push Multicast Tree

Plumtree combines low-latency tree-based routing with self-healing epidemic gossip resilience.

## Protocols
- **Eager Push (Tree)**: Disseminates full message payloads along spanning tree edges using fast point-to-point links.
- **Lazy Push (Gossip)**: Disseminates lightweight message digests (`IHAVE`) along non-tree gossip peers.
- **Pruning**: Redundant eager deliveries trigger `PRUNE` RPCs, demoting active tree links to lazy gossip links.
- **Grafting**: Missing payloads detected via lazy `IHAVE` trigger `GRAFT` messages, repairing broken tree branches.
