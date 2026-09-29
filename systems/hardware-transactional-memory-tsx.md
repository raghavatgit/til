# Hardware Transactional Memory: Intel TSX RTM and Fallback

Restricted Transactional Memory (RTM) provides hardware atomic execution blocks via CPU cache coherence protocols.

## Instructions
- `_xbegin()`: Starts speculative execution block.
- `_xend()`: Commits modified cache lines atomically.
- `_xabort()`: Aborts transaction, restoring register state.

## Fallback Design
Transactional aborts (caused by cache line eviction, interrupts, or lock conflicts) must gracefully fall back to a traditional mutex.
