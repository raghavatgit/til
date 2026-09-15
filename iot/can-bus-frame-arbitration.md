# CAN Bus Non-Destructive Bitwise Arbitration and Frame Formats

*Date: 2026-09-15*  
*Category: Embedded Systems / Automotive Telemetry*

## Overview

The Controller Area Network (CAN) bus is a robust differential serial bus standard widely deployed in automotive, aerospace, and robotics systems. Unlike Ethernet (which relies on CSMA/CD backoff retransmissions), CAN resolves bus contention **non-destructively** via bitwise arbitration.

## Physical Layer & Bus States

* **Recessive (Logic 1):** High impedance, CAN_H = 2.5V, CAN_L = 2.5V (Differential voltage = 0V).
* **Dominant (Logic 0):** Actively driven, CAN_H = 3.5V, CAN_L = 1.5V (Differential voltage = 2.0V).

**Dominant always overwrites Recessive** on the physical wire (`0 AND 1 = 0`).

## Non-Destructive Arbitration Mechanics

When multiple electronic control units (ECUs) transmit simultaneously during the arbitration field (Identifier):
1. Each transmitter samples the physical bus while outputting its identifier bit-by-bit.
2. If an ECU transmits a Recessive bit (`1`) but detects a Dominant bit (`0`) on the bus, it immediately detects that a higher-priority frame is transmitting.
3. The ECU immediately backs off and becomes a receiver without corrupting the active transmission.

```text
ECU 1 (ID 0x014 = 00000010100): Transmits '1' at bit 6 -> Reads '0' -> LOSES ARBITRATION
ECU 2 (ID 0x010 = 00000010000): Transmits '0' at bit 6 -> Reads '0' -> WINS, CONTINUES
```

Lower numerical identifier values have higher transmission priority.

## Bit Stuffing
To maintain clock synchronization across unsynchronized oscillator circuits, the transmitter automatically inserts an inverted bit after **5 consecutive identical bits**. Receivers strip stuffed bits automatically in hardware.
