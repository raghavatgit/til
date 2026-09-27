# LLVM Polly & Polyhedral Loop Optimization

## Mathematical Representation of Nested Loops
Standard compiler optimizations struggle with deeply nested loops (`for i... for j... for k...`). The polyhedral model represents nested loop iterations as integer points inside a multidimensional geometric polytope.

---

## Affine Transformations
By applying affine matrix transformations to the iteration space polytope, LLVM Polly simultaneously performs:
* **Loop Tiling:** Optimizes CPU cache reuse.
* **Loop Skewing:** Exposes hidden parallelism across iteration diagonals.
* **Loop Interchange:** Aligns memory access strides with hardware prefetchers.
