# Compact Binary Struct Packing for LoRaWAN Payloads

*Date: 2026-09-13*  
*Category: Embedded Systems / Wireless Telemetry*

## Overview

In long-range wireless sensor networks (e.g. disaster mesh deployments or agricultural telemetry), bandwidth and duty cycles are strictly constrained. Sending JSON telemetry over LoRa radio links wastes critical airtime (typically 50-100 bytes per frame) and drains battery reserves.

## The Problem

A standard JSON sensor packet:
```json
{"temp": 28.45, "humidity": 65.2, "battery": 3.92, "seq": 142}
```
Requires **64 bytes** in ASCII. Under LoRaWAN Spreading Factor 12 (SF12 / 125 kHz bandwidth), transmitting 64 bytes takes over **1.5 seconds** on-air time, violating European 1% or Indian ISM band duty cycle limits.

## The Solution: Bit-Packed Binary Structs

Packing sensor fields into fixed-precision integer offsets reduces the entire payload to **7 bytes**:

```c
#include <stdint.h>

// Packed telemetry payload (7 bytes total)
typedef struct __attribute__((packed)) {
    uint16_t seq_num;        // 2 bytes: 0 - 65535
    int16_t  temp_centi_c;   // 2 bytes: temp in C * 100 (-327.68 to +327.67 C)
    uint8_t  humidity_half;  // 1 byte : humidity % * 2 (0 - 100% in 0.5% steps)
    uint16_t battery_mv;     // 2 bytes: millivolts (e.g. 3920 mV)
} lora_telemetry_packet_t;
```

## Python Bridge Unpacker

```python
import struct

PACKET_FORMAT = "<HhBH"  # Little-endian: uint16, int16, uint8, uint16

def unpack_lora_payload(raw_bytes: bytes) -> dict:
    if len(raw_bytes) != struct.calcsize(PACKET_FORMAT):
        raise ValueError("Malformed radio payload length")
        
    seq, temp_c, hum_half, batt_mv = struct.unpack(PACKET_FORMAT, raw_bytes)
    return {
        "seq": seq,
        "temperature": temp_c / 100.0,
        "humidity": hum_half / 2.0,
        "battery_volts": batt_mv / 1000.0
    }
```

## Airtime Benchmark
* **JSON Payload (64 bytes):** 1,642 ms on-air time (SF12).
* **Binary Payload (7 bytes):** 288 ms on-air time (SF12) -> **82.4% reduction in radio airtime**.
