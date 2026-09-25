# Compilers: Link Time Optimization (LTO)

## Traditional Compilation Bottleneck
Standard compilers (GCC, Clang) compile one translation unit (`.c` or `.cpp`) at a time, emitting machine code into `.o` object files.
The linker sees only symbol names and binary offsets, making cross-module inlining and dead-code elimination impossible.

## LTO Architecture
With `-flto`:
1. Compiler frontends emit intermediate representation (LLVM bitcode or GCC GIMPLE) directly into `.o` files instead of native assembly.
2. At link time, the LTO plugin merges all translation units into a single whole-program graph.
3. Executes whole-program interprocedural optimizations:
   - Inlines critical functions across translation unit boundaries.
   - De-virtualizes C++ virtual method calls whose runtime type is statically provable.
   - Purges unused global variables, classes, and exported functions.
