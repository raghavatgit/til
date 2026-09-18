# Architecture: Memory Consistency Models (x86 TSO vs ARM Weak Ordering)

## The Hardware Motivation
CPUs execute instructions out-of-order and buffer writes in Store Buffers to hide memory bus latency.
Without memory fences, writes from one CPU core may appear in a different order to another core.

## x86 Total Store Order (TSO)
x86 processors enforce strong memory consistency:
- Loads are not reordered with other loads (Load-Load in-order).
- Stores are not reordered with earlier stores (Store-Store in-order).
- Stores are not reordered with earlier loads (Load-Store in-order).
- **The only allowed reordering**: A Store followed by a Load may be reordered (Store-Load reordering) due to store buffering.
- Result: Most acquire/release semantics in C++ on x86 translate into compiler barriers (`asm volatile("" ::: "memory")`) with zero hardware fence instructions needed except for `SeqCst` (`mfence`).

## ARM64 and RISC-V Weakly Ordered Models
ARM and RISC-V allow virtually all reorderings:
- Load-Load, Load-Store, Store-Store, and Store-Load can all reorder unless explicitly constrained.
- Hardware instructions:
  - `dmb ish`: Data Memory Barrier (Inner Shareable)
  - `dmb ishld`: Restricts load reorderings
  - Load-Acquire (`ldar`) and Store-Release (`stlr`) instructions enforce one-way barriers directly in instruction opcodes.

## C++20 Memory Order Semantics
- `memory_order_relaxed`: Atomicity without ordering guarantees.
- `memory_order_acquire`: Prevents subsequent reads/writes from moving before this load.
- `memory_order_release`: Prevents preceding reads/writes from moving after this store.
- `memory_order_seq_cst`: Enforces globally consistent total order across all threads.
