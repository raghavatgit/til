# Cache Locality in Graph Representations: Linked Lists vs CSR

*Date: 2026-09-14*  
*Category: Systems Programming / Performance Engineering*

## Overview

In textbook algorithm implementations (such as standard C adjacency lists using `malloc(sizeof(Node))`), each edge node is allocated individually on the heap:

```c
typedef struct Node {
    int dest;
    struct Node* next;
} Node;
```

While mathematically O(V + E), this representation exhibits poor memory access patterns on modern CPU architectures due to **pointer chasing**.

## The Memory Problem

1. **Spatial Locality Deficit:** Individually heap-allocated `Node` structs are scattered randomly across the 64-bit address space. Accessing `temp->next` frequently misses the CPU L1 data cache (64-byte cache line), stalling CPU execution for 50-200 cycles to fetch from main memory (DRAM).
2. **Memory Overhead:** A 32-bit integer `dest` accompanied by a 64-bit pointer `next` and 8-byte heap allocator padding consumes **24-32 bytes per edge** to store 4 bytes of actual payload (an 83% memory overhead).

## The Solution: Compressed Sparse Row (CSR)

Compressed Sparse Row (CSR) represents a graph with two contiguous flat arrays:
* **`row_ptr[V + 1]`**: Stores the starting offset in `col_idx` for each vertex.
* **`col_idx[E]`**: Stores the destination vertices in contiguous memory.

```c
typedef struct CSRGraph {
    int V;
    int E;
    int* row_ptr; // Size V + 1
    int* col_idx; // Size E
} CSRGraph;

// Traversing neighbors of vertex u in CSR:
void traverseNeighbors(CSRGraph* g, int u) {
    int start = g->row_ptr[u];
    int end = g->row_ptr[u + 1];
    for (int i = start; i < end; i++) {
        int neighbor = g->col_idx[i]; // Sequential contiguous memory access!
        process(neighbor);
    }
}
```

## Performance Benchmark
* **Contiguous Cache Lines:** The CPU hardware prefetcher detects linear array traversal and streams cache lines into L1/L2 caches ahead of execution.
* **Traversal Speedup:** CSR graphs routinely execute BFS and PageRank **3x to 8x faster** than linked-list implementations on large datasets.
