# MESI Cache Coherence Protocol and False Sharing Mitigation

## Cache Line State Transitions

In multi-socket and multi-core CPU architectures, L1/L2 caches maintain consistent views of main memory using snooping buses and directory-based coherence following the MESI protocol:

| State | Description | Shared? | Modified? |
| :--- | :--- | :--- | :--- |
| **Modified (M)** | Cache line is present only in this cache and is dirty relative to main RAM. | No | Yes |
| **Exclusive (E)** | Cache line is present only in this cache and clean (matches main RAM). | No | No |
| **Shared (S)** | Cache line may be present in multiple caches and is clean. | Yes | No |
| **Invalid (I)** | Cache line is invalid; reading requires a bus transaction. | No | No |

---

## False Sharing in Concurrent Data Structures

A cache line is typically 64 bytes. False sharing occurs when two independent threads running on different cores modify distinct variables that reside within the same 64-byte cache line:

```
Core 0 writes variable A --------+
                                  |---> 64-byte Cache Line
Core 1 writes variable B --------+
```

Each write invalidates the peer core's L1 cache line (`M -> I`), forcing continuous interconnect cache line bouncing and severe throughput collapse.

### Hardware-Level Solution: Cache Line Alignment

In Rust / C / C++:

```rust
#[repr(align(64))]
pub struct PaddedCounter {
    pub value: std::sync::atomic::AtomicU64,
}
```

Padding each core's hot mutable state to 64 bytes eliminates false sharing, restoring linear scaling across all physical CPU cores.
