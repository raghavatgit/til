# RocksDB Storage Engine Architecture

## Block Cache
RocksDB utilizes a two-level uncompressed Block Cache:
1. Data Block Cache: Stores uncompressed 4KB SSTable data blocks.
2. Filter/Index Cache: Caches SSTable two-level index structures and full Bloom filters.

## Read Path Optimization
Before touching disk, RocksDB probes Bloom filters for candidate SSTables. If the Bloom filter returns false, the key is guaranteed absent, avoiding physical I/O entirely.
