# DuckDB Vectorized Execution & Morsel-Driven Parallelism

## Vectorized Columnar Model
Unlike the Volcano iterator model (tuple-at-a-time), DuckDB processes chunks of vectors (typically 2048 values). Operations like filters and aggregations loop tightly over contiguous memory buffers, enabling CPU autovectorization (SIMD) and eliminating dynamic dispatch overhead.

---

## Morsel-Driven Parallelism
Queries dynamically partition input data into small work chunks ("morsels" of ~100k tuples). Worker threads pull morsels from a shared scheduling queue, automatically balancing load across NUMA sockets without static thread partitioning.
