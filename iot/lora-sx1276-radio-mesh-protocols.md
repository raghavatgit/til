# SX1276 LoRa RF Modulation and Multi-Hop Flood Mesh Protocols

## Physical Layer RF Parameters

Semtech SX1276/SX1278 transceivers operate using Chirp Spread Spectrum (CSS) modulation, which trades bandwidth for exceptional receiver sensitivity and high resilience against Doppler shifts and in-band interference.

### Modulation Factors

1. **Spreading Factor (SF)**:
   - Ranges from SF7 to SF12.
   - Each increment of SF doubles the chirp duration and halves the transmission bit rate while gaining ~2.5 dB in receiver sensitivity.
   - Formula: `Ts = (2^SF) / BW` where `Ts` is symbol duration and `BW` is modulation bandwidth.

2. **Bandwidth (BW)**:
   - Typically configured at 125 kHz, 250 kHz, or 500 kHz.
   - Lower bandwidth increases link budget; higher bandwidth reduces time-on-air (ToA).

3. **Coding Rate (CR)**:
   - Error-correcting Hamming code rate configured as 4/5, 4/6, 4/7, or 4/8.
   - Higher coding rates add redundancy symbols for aggressive forward error correction (FEC) at the expense of payload throughput.

---

## Controlled Multi-Hop Flooding in Emergency Networks

In GPS-denied or infrastructure-collapsed disaster zones (e.g. Aegis Raksha deployment), traditional point-to-point IP routing fails due to dynamic node churn. A controlled flooding mesh provides maximum reachability without routing table divergence.

```
[Origin Node] --(LoRa RF Broadcast)--> [Relay Node 1] --(Forward)--> [Gateway Node]
                                    \                   ^
                                     \--(Relay Node 2)--/
```

### Packet De-Duplication Protocol

To prevent broadcast storms, every frame contains:
- `Originator UUID`: 6-byte hardware identifier.
- `Message Sequence ID`: 16-bit monotonic counter.
- `Hop Count / TTL`: decremented at each forwarder; dropped when `TTL == 0`.

Relay nodes maintain an in-memory ring buffer (LRU) of recently seen `(Originator UUID, Sequence ID)` pairs. Frames matching active cache entries are dropped before transmission.
