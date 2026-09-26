# ClickHouse Sparse Primary Index & Granules

## Sparse Indexing Mechanics
Unlike traditional relational databases where B-Trees index every row, ClickHouse organizes data in compressed column files split into "granules" (default 8,192 rows).

The primary index contains only one index mark per granule. During range scans, ClickHouse binary searches the sparse index to identify matching granules, decompressing only the relevant column data blocks.
