# Storage Engines: Bloom Filter Optimization

## Purpose
LSM storage engines (RocksDB, LevelDB) maintain a Bloom filter per SSTable file to avoid costly disk I/O for non-existent keys.

## Math & Invariants
Given $m$ bits and $n$ inserted keys, optimal number of hash functions $k$:
$$k = \frac{m}{n} \ln 2 \approx 0.693 \frac{m}{n}$$
False positive probability $p$:
$$p \approx (1 - e^{-kn/m})^k$$
At 10 bits per key ($m/n = 10$), $k \approx 7$, resulting in $p \approx 0.008$ (~0.8% false positive rate).

## Hash Efficiency (Kirsch-Mitzenmacher Technique)
Simulate $k$ independent hash functions using only 2 hash invocations:
$$g_i(x) = h_1(x) + i \times h_2(x) \pmod m$$
