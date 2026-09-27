# Columnar Compression: RLE & Dictionary Encoding

## Physical Layout in Parquet & ORC
Columnar storage groups values of the same column into contiguous memory blocks, exposing massive structural redundancy.

---

## Compression Techniques
* **Dictionary Encoding:** Maps unique column strings to small integer IDs (e.g. 1-byte integers). Used when cardinality is low.
* **Run-Length Encoding (RLE):** Compresses repeated sequential values into `(value, count)` pairs.
* **Bit-Packing:** Stores 3-bit integers tightly across 64-bit words without padding to whole byte boundaries, quadrupling memory bandwidth utilization.
