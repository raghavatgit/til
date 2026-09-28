# CPU Cache False Sharing and Cache-Line Alignment

## Cache Coherence Protocol (MESI / MOESI)
CPUs transfer memory between L1/L2/L3 caches and RAM in units of Cache Lines (typically 64 bytes on modern x86_64 and ARM64).

## False Sharing Problem
False sharing occurs when two threads running on distinct CPU cores modify independent variables that reside within the exact same 64-byte cache line. Even though the variables are logically decoupled:
1. Core A writes to `varA`, invalidating the entire cache line across all other cores.
2. Core B attempts to read or write `varB`, incurring an L1 cache miss and forcing cache-line transfer via inter-core interconnect (QPI/UPI/Infinity Fabric).

## Mitigation
- Add padding or use compiler attributes: `alignas(64)` in C++ or `#[repr(align(64))]` in Rust.
- Keep thread-local counters separate and aggregate only during collection phases.
