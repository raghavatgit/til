# eBPF: Verifier Safety and JIT Execution

## Overview
Extended Berkeley Packet Filter (eBPF) executes custom sandboxed bytecode inside the Linux kernel in response to tracepoints, kprobes, uprobes, and network packets.

## Verifier Safety Verification
Before loading bytecode into the kernel, the eBPF verifier checks mathematical proofs:
1. **Control Flow Graph (CFG) Analysis**: Ensures program termination (bounded loops, no unreachable instructions).
2. **Memory Safety**: Verifies that every pointer dereference stays within valid bounds (no arbitrary kernel memory access).
3. **Type Checking**: Ensures context structures match hook types.
4. **Register State Tracking**: Verifies initialization of registers before read.

## JIT Compilation
Once approved, the in-kernel JIT compiler converts the 64-bit eBPF bytecode directly into native host machine instructions (x86_64, ARM64) with near-zero overhead.
