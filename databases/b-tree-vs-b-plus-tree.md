# Storage Engines: B-Tree vs B+ Tree

## Structural Divergence
- **B-Tree**: Stores both search keys and associated data/record pointers in all nodes (internal and leaf).
- **B+ Tree**: Internal nodes store only indexing keys and child page pointers. All actual payload data resides exclusively in leaf nodes. Leaf nodes are linked via a doubly linked list.

## Why Databases Almost Exclusively Use B+ Trees
1. **Higher Branching Factor**: Internal pages contain no data payloads, allowing hundreds of keys per 4KB/8KB page, reducing tree depth.
2. **Sequential Range Scans**: Range queries scan sequentially across linked leaf pages without traversing back up through parent nodes.
3. **Predictable Query Latency**: Every lookup requires traversing exactly $H$ levels to a leaf node.
