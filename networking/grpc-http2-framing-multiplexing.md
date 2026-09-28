# gRPC Protocol Wire Serialization and HTTP/2

## Protocol Foundation
gRPC operates exclusively over HTTP/2, leveraging binary framing and multiplexed connections.

## Wire Layout of a gRPC Message
A gRPC payload starts with a 5-byte prefix followed by the serialized Protocol Buffers message:
- 1 byte: Compressed Flag (`0` = uncompressed, `1` = compressed via gzip/snappy).
- 4 bytes: Message Length (Big-Endian 32-bit unsigned integer).
- N bytes: Serialized Protobuf bytes.

## Status Codes and Trailers
Unlike traditional REST APIs that return HTTP 200/400/500, gRPC always returns HTTP 200 OK headers if the transport succeeded, conveying RPC status (`grpc-status: 0` for OK, `14` for UNAVAILABLE) and error details via HTTP/2 trailing headers (`END_STREAM`).
