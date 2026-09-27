# ZFS Copy-on-Write & Adaptive Replacement Cache (ARC)

## The Uberblock & Never Overwrite Rule
ZFS is a transactional Copy-on-Write (COW) filesystem. Data blocks are never modified in-place:
1. New writes allocate fresh disk sectors.
2. Parent indirect blocks are duplicated up the Merkle tree to the root Uberblock.
3. Atomic flush of the Uberblock commits the transaction.

---

## Adaptive Replacement Cache (ARC)
ZFS replaces simple Least Recently Used (LRU) caching with ARC:
* Dynamically balances between Frequency (LFU) and Recency (LRU).
* Adapts cache capacity automatically between streaming workloads and repeated index queries.
