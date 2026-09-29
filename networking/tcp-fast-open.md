# TCP Fast Open (TFO - RFC 7413)

## Standard TCP Handshake Overhead
Standard TCP requires a full 1-RTT handshake (SYN -> SYN-ACK -> ACK + Data) before transmitting application data.

## TFO Mechanism
1. First Connection: Client sends SYN with a TFO option requesting a Cookie. Server replies with SYN-ACK containing an encrypted TFO Cookie.
2. Subsequent Connections: Client sends SYN containing both the Cookie and initial application payload (e.g., HTTP GET request). Server validates cookie and immediately passes data to userspace, achieving 0-RTT data delivery.
