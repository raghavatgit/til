# Operating Systems: Linux Page Cache Speculative Readahead and posix_fadvise

## The Sequential Readahead Window
When an application reads a file sequentially:
- Linux kernel detects monotonic offset progression and initiates speculative readahead:
  - Allocates pages in the Page Cache ahead of the application's current file pointer.
  - Submits asynchronous DMA block requests to storage hardware.
- If application reads match predicted offsets, reads complete with zero I/O latency directly from RAM.

## posix_fadvise(2) Hints
Applications can explicitly guide kernel page caching:
- `POSIX_FADV_SEQUENTIAL`: Doubles default readahead window size.
- `POSIX_FADV_RANDOM`: Disables speculative readahead entirely (prevents loading unused 128KB chunks on random B-Tree seeks).
- `POSIX_FADV_WILLNEED`: Asynchronously triggers immediate page cache pre-population.
- `POSIX_FADV_DONTNEED`: Evicts pages from RAM immediately (used by streaming backup daemons to avoid polluting active memory).
