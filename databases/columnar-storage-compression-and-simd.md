# Storage: Columnar Storage Compression, Dictionary Encoding, and SIMD

## Row-Oriented vs Columnar Encoding
- Row-Oriented (CSV, SQLite): Contiguous records `[Name, Age, Salary, Name, Age, Salary]`.
- Columnar (Parquet, ORC, ClickHouse): Homogeneous values stored contiguously `[Age, Age, Age...]`.

## Columnar Compression Primitives
1. **Dictionary Encoding**: Replaces repetitive string values with small integer IDs (e.g. 1-2 bytes).
2. **Run-Length Encoding (RLE)**: Replaces repeated identical values with `(count, value)` tuples.
3. **Bit-Packing**: Packing integer values that fit within $k$ bits into contiguous 64-bit words without padding.

## SIMD Fast Bit-Unpacking
Decompressing bit-packed columns using AVX2:
- Vector register loads 256 bits at once.
- Shift and mask operations (`_mm256_sllv_epi32`, `_mm256_and_si256`) unpack 8 x 32-bit integers in parallel in a single CPU clock cycle.
- Delivers query scan rates exceeding 10 billion integers per second per CPU core.
