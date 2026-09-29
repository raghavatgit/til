# CPU Memory Barriers and Store Buffers

## The Store Buffer
To hide L1 cache write latencies, CPU cores write into hardware Store Buffers. Without barriers, writes to memory can be reordered relative to subsequent reads (x86 TSO memory model).

## Instructions
- `LFENCE`: Disallows speculative execution past the fence and orders load operations.
- `SFENCE`: Flushes the store buffer, ordering store operations.
- `MFENCE`: Full bidirectional memory barrier ordering all loads and stores.
