# High-Throughput S3 Multipart Uploads

## Invariants
- Minimum Part Size: 5 MB (except the last part).
- Maximum Part Size: 5 GB.
- Maximum Parts: 10,000.
- Single Object Maximum: 5 TB.

## Concurrency
Uploading parts in parallel across 10 - 20 worker threads saturates 10GbE network interfaces, maximizing cloud object throughput.
