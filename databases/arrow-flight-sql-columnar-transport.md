# Apache Arrow Flight SQL Columnar Transport

Arrow Flight SQL utilizes gRPC HTTP/2 transport to transfer Apache Arrow record batches directly across network sockets without row serialization.

## Performance Drivers
1. **Zero-Copy Serialization**: CPU registers operate directly on Arrow memory buffers.
2. **Parallel Streams**: Query tickets allow clients to consume partitioned query results from multiple workers concurrently.
3. **Dictionary Encoding**: Transmits categorical strings as compact integer indices.
