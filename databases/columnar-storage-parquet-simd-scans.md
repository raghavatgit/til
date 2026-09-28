# Columnar Storage: Parquet and Vectorized SIMD Scanning

## Row-Oriented vs Column-Oriented
- **Row-Oriented (OLTP)**: Entire tuple stored contiguously on disk. Efficient for single-row lookups (`SELECT * WHERE id = 123`).
- **Column-Oriented (OLAP)**: Each column stored in contiguous disk blocks. Efficient for aggregations (`SELECT AVG(revenue) FROM sales`).

## Apache Parquet Format
- File organized into Row Groups (typically 128 MB to 512 MB).
- Each Row Group contains Column Chunks, subdivided into Pages.
- **Dremel Encoding**: Supports nested data via Definition Levels and Repetition Levels.
- **Compression**: Run-Length Encoding (RLE) and Dictionary Encoding achieve 80%+ compression ratios.
- **SIMD Processing**: Contiguous numerical column data loads directly into 256-bit (AVX2) and 512-bit (AVX-512) registers for parallel instruction processing.
