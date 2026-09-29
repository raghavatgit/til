# False Sharing and Cache Line Alignment

## Mechanism
Modern x86 and ARM CPUs synchronize memory in 64-byte cache lines via hardware coherence protocols (MESI/MOESI). If thread 1 modifies variable A and thread 2 modifies variable B on the same 64-byte line, each write forces an invalidation storm across CPU interconnects, degrading throughput by up to 20x.

## Solution in Systems Languages
- C/C++: `alignas(64) struct PaddedCounter { uint64_t val; };`
- Rust: `#[repr(align(64))] struct CachePadded<T>(T);`
