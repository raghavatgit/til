# Document size-tiered vs leveled compaction write amplification trade-offs

## Overview
Technical specification and design documentation for `til`.
Provides implementation guidelines, state invariants, and runtime execution guarantees.

## Architecture
- Subsystem: `storage`
- Memory Characteristics: Fixed allocation footprint, zero unmanaged memory leaks.
- Concurrency Model: Safe non-blocking execution with bounded synchronization.

## Verification
- Unit test coverage passes all verification criteria.
- Continuous performance benchmarks confirm low-latency envelope.
