# DuckDB: Vectorized Execution and Morsel-Driven Parallelism

## Volcano Iterator vs Vectorized Execution
- **Volcano (Tuple-at-a-time)**: Virtual function calls per row induce severe instruction cache misses and branch mispredictions.
- **Vectorized (Block-at-a-time)**: Operates on vectors of 2048 values contiguously in memory, enabling CPU cache residency and SIMD instructions.

## Morsel-Driven Parallelism
- Work divided into "morsels" (~100,000 tuples).
- Worker threads dynamically pull morsels from a central pipeline dispatcher, achieving perfect load balancing across uneven CPU core availability without rigid static partition splits.
