# Rust Miri: Undefined Behavior & Stacked Borrows Validation

## Interpreting Rust Mid-level IR (MIR)
Miri is an interpreter for Rust's Mid-level Intermediate Representation (MIR) designed to detect Undefined Behavior (UB) in `unsafe` code before compilation to native machine code.

---

## Stacked Borrows & Tree Borrows
Miri validates Rust's pointer aliasing model:
* Every memory allocation maintains a stack of active loans/pointers.
* Accessing memory via an invalid pointer pops parent loans, flagging invalid writes or dangling raw pointers.
* Detects unaligned memory reads, uninitialized memory usage, and data races.
