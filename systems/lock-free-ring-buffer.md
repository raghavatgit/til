# Lock-Free SPSC Ring Buffer Architecture

## Invariant
- Single producer writes to `head` and reads `tail`.
- Single consumer reads from `tail` and reads `head`.
- Head and tail pointers are cacheline-aligned (`alignas(64)`) to eliminate false sharing.

## C++ Implementation
```cpp
template <typename T, size_t Capacity>
class SPSCQueue {
    static_assert((Capacity & (Capacity - 1)) == 0, "Capacity must be power of two");
    T buffer[Capacity];
    alignas(64) std::atomic<size_t> head{0};
    alignas(64) std::atomic<size_t> tail{0};

public:
    bool push(const T& item) {
        size_t h = head.load(std::memory_order_relaxed);
        size_t t = tail.load(std::memory_order_acquire);
        if (h - t == Capacity) return false; // Full
        buffer[h & (Capacity - 1)] = item;
        head.store(h + 1, std::memory_order_release);
        return true;
    }

    bool pop(T& item) {
        size_t t = tail.load(std::memory_order_relaxed);
        size_t h = head.load(std::memory_order_acquire);
        if (h == t) return false; // Empty
        item = buffer[t & (Capacity - 1)];
        tail.store(t + 1, std::memory_order_release);
        return true;
    }
};
```
