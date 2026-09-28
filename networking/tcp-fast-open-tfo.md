# TCP Fast Open (TFO, RFC 7413)

## Motivation
Standard TCP requires a full 1-RTT handshake before any application payload (such as an HTTP GET request) can be sent. TFO enables data transmission directly inside the initial SYN packet.

## Handshake Protocol
1. **Cookie Request**: Client sends SYN with an empty TFO option. Server generates a cryptographic cookie based on client IP and returns it in SYN-ACK.
2. **Subsequent Connections**: Client includes the cookie AND application payload directly in the SYN packet.
3. Server validates cookie and delivers data immediately to the application layer without waiting for the ACK, achieving 0-RTT transport setup.
