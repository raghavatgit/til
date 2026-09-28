# Bloom Filters in LSM-tree SSTables

## The Lookup Problem
In an LSM storage engine (RocksDB, LevelDB), reading a non-existent key requires checking SSTables across every level, incurring catastrophic random I/O.

## Bloom Filter Algorithm
A space-efficient probabilistic data structure:
- Bit array $m$ initialized to all 0s.
- $k$ independent hash functions.
- Insertion: Hash key with $k$ functions and set corresponding bits to 1.
- Query: If any of the $k$ bit positions is 0, the key **definitely does not exist** in the SSTable. If all are 1, the key **probably exists**.

## Optimal Parameter Sizing
For target false-positive probability $p$:
$$m = -\frac{n \ln p}{(\ln 2)^2}, \quad k = \frac{m}{n} \ln 2$$
Using 10 bits per key yields a false positive rate of $\sim 1\%$, avoiding 99% of pointless disk seeks.
