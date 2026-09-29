# Prefix Bloom Filters in RocksDB Range Scans

## Standard Bloom Filter Limitation
Standard Bloom Filters hash the entire key string. They cannot accelerate range scans (`Iterator::Seek("user:1001:*")`) because full key hashes do not preserve lexicographical prefixes.

## Prefix Bloom Filter Mechanism
1. Define a `SliceTransform` extractor (e.g., first 8 bytes or fixed delimiter).
2. During SSTable generation, RocksDB hashes only the extracted prefix into the Bloom Filter.
3. On `Seek(prefix)`, RocksDB checks if the prefix exists in the SSTable Bloom Filter, skipping entire SSTable files that contain no keys matching the search prefix.
