# Direct I/O (`O_DIRECT`) and Page Cache Bypass

## Motivation
The Linux Page Cache buffers disk reads and writes in RAM. For database engines implementing their own buffer pools (e.g. PostgreSQL, MySQL InnoDB), double buffering wastes memory and induces double copy overhead.

## Buffer Alignment Invariants
`O_DIRECT` requires:
1. Memory buffer address must be aligned to the physical block size (512 or 4096 bytes).
2. File offset must be a multiple of the block size.
3. Transfer length must be a multiple of the block size.
