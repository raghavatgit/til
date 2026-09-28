# Chandy-Lamport Distributed Snapshot Algorithm

## Purpose
Captures a globally consistent state of a distributed system (both process local memory and in-flight communication channel messages) without halting execution.

## Marker Propagation Rules
1. **Initiator**: Process records its own state and sends a `Marker` token along all outgoing channels.
2. **Receiver**: On receiving marker along channel $C$:
   - If process has not recorded its state: records state, marks channel $C$ as empty, and broadcasts marker along all its outgoing channels.
   - If process has already recorded state: records all messages arriving on channel $C$ until the marker arrives as the in-flight state of $C$.
