# QUIC: Loss Detection and Adaptive ACK Frequency

## Packet Number Spaces
QUIC uses monotonic, never-reused packet numbers across 3 independent encryption spaces (Initial, Handshake, Application Data), preventing ambiguous ACK round-trip time measurements.

## Loss Detection Heuristics
1. **Packet Threshold**: If packet $N + 3$ is acknowledged, packet $N$ is declared lost.
2. **Time Threshold**: If packet $N$ was sent $> 9/8 \times \text{RTT}$ ago and a newer packet is acknowledged, packet $N$ is declared lost.

## Adaptive ACK Frequency (RFC draft)
Instead of acknowledging every second packet, endpoints negotiate ACK frequency dynamically (e.g., once every 10 packets or once every 25ms), drastically reducing return path upload bandwidth on asymmetrical connections.
