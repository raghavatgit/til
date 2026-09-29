# mimalloc: Thread-Local Sharded Free Lists Architecture

Microsoft's `mimalloc` achieves high allocation throughput via page-aligned heap architectures and thread-local free list slicing.

## Allocator Design
- **Local Free List**: Fast path allocation entirely without atomic CAS operations.
- **Thread Free List**: Remote cross-thread deallocations pushed via atomic CAS and reclaimed in batches.
- **Page Spans**: Fixed size-class bins eliminate memory fragmentation.
