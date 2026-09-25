# Storage Systems: Columnar File Formats (Parquet & ORC)

## Row-Oriented vs Column-Oriented
- **Row-Oriented (CSV, JSON, Avro)**: Stores contiguous fields of a single record together. Optimal for OLTP point lookups and single-row insertions.
- **Column-Oriented (Parquet, ORC)**: Stores all values of a given column contiguously on disk. Optimal for analytical OLAP aggregation queries (`SELECT AVG(price) FROM sales`).

## Compression Advantages
Because column values share identical data types and similar distributions:
1. **Dictionary Encoding**: Replaces repetitive strings with compact integer IDs.
2. **Run-Length Encoding (RLE)**: Encodes consecutive identical values as `(count, value)`.
3. **Bit-Packing**: Uses only the exact number of bits required to store the maximum integer.

## Predicate Pushdown
Parquet row groups embed column metadata (minimum and maximum value per chunk).
Engines skip reading entire 100MB row groups off disk if `filter_val < min || filter_val > max`.
