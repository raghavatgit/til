# QUIC Latency Spin Bit & Passive RTT Measurement

## Visibility in Encrypted Protocols
Because QUIC encrypts transport headers, network operators cannot observe TCP sequence/ACK numbers to calculate network round-trip time (RTT).

---

## The Spin Bit Mechanism (RFC 9000)
QUIC reserves a single bit in the short header:
1. **Client Initiation:** Client transmits packets with spin bit `0`.
2. **Server Reflection:** When server receives a packet with spin bit `0`, it reflects `0` back in response packets for the current RTT.
3. **Client Flip:** Once client receives the reflected bit, it flips the spin bit to `1` for subsequent packets.
4. Passive observers on the network path measure the time interval between observed bit flips, calculating exact end-to-end RTT without decrypting payload data.
