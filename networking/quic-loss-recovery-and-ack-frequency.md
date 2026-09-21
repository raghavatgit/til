# Networking: QUIC Packet Number Spaces and ACK Frequency Optimization

## Monotonic Packet Numbers (No TCP-Style Sequence Wrapping)
In TCP:
- Retransmitted segments reuse the same sequence number.
- Ambiguity: Was an ACK generated in response to the original packet or the retransmission?

In QUIC (RFC 9002):
- Every packet receives a strictly monotonically increasing packet number, including retransmissions.
- Eliminates retransmission ambiguity entirely, enabling precise Round-Trip Time (RTT) estimation.

## Distinct Packet Number Spaces
QUIC maintains 3 independent packet number spaces:
1. **Initial**: Unprotected handshake tokens.
2. **Handshake**: Encrypted with handshake secrets.
3. **Application Data**: 1-RTT encrypted application payloads.
Packet loss in the Handshake space never halts or delays processing of Application Data packets.
