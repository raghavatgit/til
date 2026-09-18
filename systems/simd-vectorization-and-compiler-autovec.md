# Systems: SIMD Vectorization, Cache Alignment, and Autovectorization

## Vector Register Widths
Single Instruction, Multiple Data (SIMD) processes contiguous data lanes in parallel:
- SSE: 128-bit registers (`xmm0`-`xmm15`, 4 x 32-bit floats).
- AVX2: 256-bit registers (`ymm0`-`ymm15`, 8 x 32-bit floats).
- AVX-512: 512-bit registers (`zmm0`-`zmm31`, 16 x 32-bit floats).
- ARM NEON: 128-bit vector registers (`v0`-`v31`).

## Data Alignment Invariants
Unaligned loads (`_mm256_loadu_ps`) cross cache line boundaries (64 bytes), requiring two memory requests and line splitting:
- Aligned loads (`_mm256_load_ps`) require 32-byte boundary alignment (`alignas(32)`).
- Cross-boundary penalties drop throughput significantly in compute-bound inner loops.

## Compiler Autovectorization Invariants
To ensure GCC/Clang autovectorize loops without falling back to scalar code:
1. **No Loop Dependencies**: Avoid loop-carried dependencies where iteration $i$ reads data modified by iteration $i - 1$.
2. **Pointer Non-Aliasing**: Use `__restrict` pointers to guarantee input and output buffers do not overlap.
3. **Trip Count Known or Simple**: Avoid loop breaks or early returns inside vector loops.
