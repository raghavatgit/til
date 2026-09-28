# CPU Memory Consistency Models and Memory Barriers

## Total Store Order (TSO) vs Weak Ordering
- **x86_64 (TSO)**: Enforces strict order on Store-Store, Load-Load, and Load-Store operations. The only permissible reordering is Store-Load (a store buffered in the store buffer may be delayed past a subsequent load).
- **ARM64 / RISC-V (Weakly Ordered)**: Hardware can reorder loads and stores arbitrarily unless explicit memory barriers are emitted.

## Memory Barriers
1. `smp_mb()`: Full memory barrier (prevents reordering across barrier).
2. `smp_rmb()`: Read memory barrier (orders loads).
3. `smp_wmb()`: Write memory barrier (orders stores).
4. Acquire/Release semantics:
   - Acquire: Ensures no subsequent read/write can be moved before this operation.
   - Release: Ensures no prior read/write can be moved after this operation.
