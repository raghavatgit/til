# TCP RACK: Time-Based Loss Detection (RFC 8985)

## Flaws in Duplicate ACK Counting
Classic TCP loss detection triggers fast retransmit after receiving 3 duplicate ACKs (DupACKs). In modern high-bandwidth networks, minor packet reordering or small flight sizes falsely trigger congestion collapse and halved congestion windows.

---

## RACK Time-Based Invariant
Recent ACKnowledgment (RACK) uses time rather than packet counts:
* If packet `X` was transmitted before packet `Y`, and packet `Y` has been acknowledged (via SACK), then packet `X` is marked lost if `time_since(X) > RTT + reordering_window`.
* Operates effectively during tail drops, small windows, and reordered wireless paths.
