# Linux TCP Small Queues (TSQ) and Autocorking

The Linux networking stack implements TSQ and autocorking to prevent bufferbloat in network interface driver queues.

## Mechanisms
- **TSQ**: Limits the byte quantity of unacknowledged packets queued in device qdiscs per socket (default 1-2 packets).
- **Autocorking**: Delays emitting small data packets if a packet is already in flight on the wire, amalgamating user payload into single MSS frames.
