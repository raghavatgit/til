# HTTP/3 WebTransport and Unreliable Datagrams

WebTransport extends HTTP/3 and QUIC to provide low-latency unreliable datagrams alongside multiplexed reliable streams.

## Features
- Eliminates head-of-line blocking for real-time video/gaming telemetry.
- Shares single TLS 1.3 handshake and congestion control context across datagrams and byte streams.
