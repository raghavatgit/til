# ESP32 Serial Packet Framing and Telemetry Buffering

*Date: 2026-09-08*  
*Category: Embedded Systems / IoT*

## Overview

When streaming high-frequency microclimate sensor metrics from an ESP32 microcontroller to a host Python service (such as in the Agrostat bridge), serial reads often return fragmented bytes or merged chunks. Reading raw serial lines directly without explicit delimiter boundaries leads to JSON decode exceptions and dropped frames.

## The Problem

Host serial buffers fill asynchronously relative to Python's read loop. A single `serial.read()` or `serial.readline()` call may capture:
1. Partial packets (e.g. `{"temp": 28.4, `)
2. Multiple concatenated packets (e.g. `{"temp": 28.4}\n{"temp": 28.5}\n`)
3. Corrupted bytes during USB reconnect cycles.

## Robust Buffering Architecture

Using an accumulator buffer with strict newline boundary splitting ensures atomic packet parsing:

```python
import serial
import json

def stream_telemetry_packets(port: str, baud: int = 115200):
    ser = serial.Serial(port, baud, timeout=0.1)
    buffer = bytearray()
    
    while True:
        chunk = ser.read(ser.in_waiting or 1)
        if not chunk:
            continue
            
        buffer.extend(chunk)
        
        while b"\n" in buffer:
            packet_raw, buffer = buffer.split(b"\n", 1)
            line = packet_raw.strip()
            
            if not line:
                continue
                
            try:
                data = json.loads(line.decode("utf-8", errors="ignore"))
                yield data
            except json.JSONDecodeError:
                # Discard corrupted telemetry frame safely
                continue
```

## Key Principles

* **Explicit Framing:** Never rely on fixed byte lengths for variable telemetry payloads. Use delimiter-guarded frames.
* **Non-Blocking In-Waiting:** Check `ser.in_waiting` to drain buffered bytes without blocking the main event loop.
* **Graceful Degradation:** Malformed frames are logged and discarded without halting serial bridge operations.
