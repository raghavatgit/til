# Cache-Padded SPSC Lock-Free Ring Buffer

## Motivation
Standard cross-thread message passing channels rely on OS futexes or mutex synchronization, inducing thread descheduling and context switch latencies (upwards of 1.5 - 3.0 microseconds). A Single-Producer Single-Consumer (SPSC) lock-free ring buffer achieves sub-10ns latency using atomic operations and cache-line isolation.

## Memory Model and Cache Alignment
False sharing occurs when the producer write index and consumer read index reside on the same 64-byte CPU cache line, triggering cache invalidation traffic across CPU interconnects (MESI protocol invalidation).

```rust
use std::sync::atomic::{AtomicUsize, Ordering};

#[repr(align(64))]
pub struct CachePaddedIndex {
    value: AtomicUsize,
}

pub struct SpscRingBuffer<T, const CAP: usize> {
    buffer: [Option<T>; CAP],
    head: CachePaddedIndex, // Read index (consumer exclusive)
    tail: CachePaddedIndex, // Write index (producer exclusive)
}
```

## Ordering Invariants
- Producer uses `Ordering::Release` when publishing new items to guarantee payload visibility.
- Consumer uses `Ordering::Acquire` when loading the tail index to observe fully published payloads.
