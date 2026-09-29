# Brotli vs Zstandard HTTP Compression Analysis

## Key Trade-Offs
- Brotli: Employs a pre-defined static dictionary of 13,000 common HTML/CSS/JS strings. Achieves 15-20% higher compression ratio on static web text compared to Gzip.
- Zstandard: Developed by Meta. Features ultra-fast decompression (> 1.2 GB/s per core) and adaptive compression levels (1 - 22), ideal for real-time streaming and API payloads.
