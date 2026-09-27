# LSM Tree Prefix Bloom Filters

## Standard Bloom Filter Limitation
Standard Bloom filters only answer point lookup existence queries: `Does key K exist in SSTable?`. They cannot filter range scans (`SEEK [K1, K2]`), forcing range queries to touch every SSTable across all LSM levels.

---

## Prefix Bloom Filter Mechanism
RocksDB and Pebble support Prefix Bloom Filters:
1. Keys are partitioned into `[prefix]:[suffix]` (e.g. `user_102:orders_2026`).
2. The Bloom filter hashes only the `prefix` component.
3. When executing a range scan with a fixed prefix, the engine queries the prefix Bloom filter to bypass SSTables that contain no keys matching that prefix.
