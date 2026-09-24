# Memory Ordering: Acquire-Release Semantics

## Memory Models
- **`memory_order_relaxed`**: Guarantees atomic read/write, but allows arbitrary compiler and hardware reordering.
- **`memory_order_acquire`**: No subsequent read or write in current thread can be reordered before this load. Synchronizes with an `atomic_store_release`.
- **`memory_order_release`**: No prior read or write in current thread can be reordered after this store.
- **`memory_order_seq_cst`**: Globally coherent total store order across all cores (highest overhead due to hardware store buffer flushes).

## Publisher-Subscriber Pattern
```cpp
// Publisher thread
data = 42;
ready.store(true, std::memory_order_release);

// Subscriber thread
if (ready.load(std::memory_order_acquire)) {
    assert(data == 42); // Guaranteed to observe data = 42
}
```
