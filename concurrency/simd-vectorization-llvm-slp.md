# SIMD Auto-Vectorization & LLVM SLP Internals

## Overview: Single Instruction Multiple Data
Modern x86-64 (AVX-2, AVX-512) and ARM (NEON, SVE) CPUs feature vector execution units capable of operating on 128-bit, 256-bit, or 512-bit registers. Rather than processing one 32-bit float per cycle, an AVX-2 unit processes eight 32-bit floats simultaneously in a single clock cycle.

---

## Two Vectorization Engines in LLVM

1. **Loop Vectorizer**:
   - Transforms sequential loops into vectorized SIMD instructions.
   - Calculates loop trip counts, strips loop pre-headers/tails for alignment, and generates stride-based vector operations.

2. **SLP Vectorizer (Superword-Level Parallelism)**:
   - Identifies isomorphic operations across straight-line basic blocks (e.g. assigning `x`, `y`, `z`, `w` vector coordinates).
   - Combines independent scalar operations of the same type into packed vector registers.

---

## Idiomatic Vectorization Patterns in Rust

```rust
// Autovectorizer-friendly: contiguous slice, no branches, known boundary
pub fn vector_add_simd(a: &[f32], b: &[f32], out: &mut [f32]) {
    assert_eq!(a.len(), b.len());
    assert_eq!(a.len(), out.len());

    // Compiler automatically unrolls and emits VADDPS (AVX) or fadd (NEON)
    for i in 0..a.len() {
        out[i] = a[i] + b[i];
    }
}
```

### Rules for Guaranteed Auto-Vectorization
* **Eliminate Inner Loop Branches:** Conditionals cause vector divergence. Replace `if-else` with branchless select/min/max primitives.
* **Guarantee Memory Disjointness:** Use slice bounds checks or `#[inline]` to prove pointers do not alias (`noalias` attribute in LLVM IR).
* **Memory Alignment:** 32-byte or 64-byte aligned buffers allow aligned vector loads (`VMOVAPS`) without unaligned memory penalties.
