# ClickHouse: MergeTree Storage Engine Internals

## Physical Data Layout
Data is divided into immutable directory parts (`part_001_001_0`). Each part contains:
- `column.bin`: Compressed column chunks.
- `column.mrk`: Mark files indexing block offsets.
- `primary.idx`: Sparse primary index.

## Sparse Indexing
- Does not index every row (unlike B+ Trees).
- Indexes one row per index granularity block (default 8192 rows).
- Query execution reads the lightweight sparse index into RAM, binary searches mark ranges, and streams only matching compressed data blocks directly from disk.
