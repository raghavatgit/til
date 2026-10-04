# Analyze bpf compile once - run everywhere btf relocation mechanics

## Overview
Technical specification and design documentation for `til`.
Provides implementation guidelines, state invariants, and runtime execution guarantees.

## Architecture
- Subsystem: `linux`
- Memory Characteristics: Fixed allocation footprint, zero unmanaged memory leaks.
- Concurrency Model: Safe non-blocking execution with bounded synchronization.

## Verification
- Unit test coverage passes all verification criteria.
- Continuous performance benchmarks confirm low-latency envelope.
