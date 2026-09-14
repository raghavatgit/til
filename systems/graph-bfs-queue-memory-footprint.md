# Memory Footprint and Queue Sizing in Graph BFS Traversals

*Date: 2026-09-14*  
*Category: Systems Programming / Memory Optimization*

## Overview

In Breadth-First Search traversals on large memory graphs (e.g. social network degrees of separation, web crawlers), the FIFO queue stores the entire frontier boundary of vertices at depth `k`.

## The Memory Frontier Problem

* In a tree or graph with branching factor `b`, the queue size at depth `d` scales as `O(b^d)`.
* For a graph with `V = 10,000,000` vertices, dynamic queue reallocations (`std::vector::push_back` or unbounded `VecDeque`) can trigger memory pressure and garbage collection pauses.

## Circular Ring Buffer Queue

Using a statically allocated circular ring buffer of capacity `V` eliminates dynamic reallocations:

```c
typedef struct {
    int* buffer;
    int head;
    int tail;
    int capacity;
} RingQueue;

void initQueue(RingQueue* q, int cap) {
    q->buffer = (int*)malloc(cap * sizeof(int));
    q->head = 0;
    q->tail = 0;
    q->capacity = cap;
}

static inline void push(RingQueue* q, int val) {
    q->buffer[q->tail] = val;
    q->tail = (q->tail + 1) % q->capacity;
}

static inline int pop(RingQueue* q) {
    int val = q->buffer[q->head];
    q->head = (q->head + 1) % q->capacity;
    return val;
}
```

## Key Advantage
Circular ring buffers maintain constant contiguous heap space with zero heap fragmentation and predictable memory bounds during traversal.
