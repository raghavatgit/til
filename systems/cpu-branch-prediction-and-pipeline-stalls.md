# CPU Branch Prediction, Pipeline Bubbles, and Branchless Code

*Date: 2026-09-15*  
*Category: Systems Programming / Computer Architecture*

## Overview

Modern out-of-order x86_64 CPUs feature deep execution pipelines (typically 14 to 20 stages). When conditional branching instructions (`je`, `jne`) occur, the CPU cannot wait for instruction retirement to know which path to fetch next. Instead, Branch Prediction Units (Branch Target Buffers and 2-level adaptive predictors) speculatively guess the path.

## The Cost of Branch Misprediction

When a branch guess is incorrect:
1. All speculatively decoded and dispatched micro-ops in flight must be flushed.
2. The pipeline is cleared, generating **pipeline bubbles**.
3. **Penalty:** 15 to 20 CPU cycles per misprediction. In tight loops (e.g. sorting, raymarching, audio DSP), mispredictions degrade throughput by 2x to 5x.

## Branchless Programming Pattern

Instead of branches:
```cpp
// Branching version (unpredictable on random data)
int max_branch(int a, int b) {
    if (a > b) return a;
    return b;
}
```

Use branchless conditional moves (`cmov` on x86):
```cpp
// Branchless version: compiles to 'cmp' followed by 'cmovg'
int max_branchless(int a, int b) {
    return (a > b) ? a : b; // Modern GCC/Clang emits cmov without jmp
}
```

Or arithmetic masking:
```c
// Branchless absolute value
int abs_branchless(int n) {
    int mask = n >> 31; // -1 if negative, 0 if positive
    return (n + mask) ^ mask;
}
```

## Takeaway
When processing random or unsorted datasets in high-performance computing, restructuring branches into conditional moves (`cmov`) or bitwise masks keeps CPU instruction pipelines fully saturated.
