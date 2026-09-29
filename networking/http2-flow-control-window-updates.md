# HTTP/2 Flow Control: WINDOW_UPDATE Frames

## Dual-Layer Flow Control
Unlike HTTP/1.1, HTTP/2 manages flow control at two distinct granularities:
1. **Connection Level**: Bounded total bytes allowed across all concurrent streams.
2. **Stream Level**: Bounded bytes allowed on each individual logical stream.

## Mechanics
- Both client and server advertise initial window sizes (default: 65,535 bytes).
- Sending `DATA` frames decrements both the stream window and the connection window.
- When a receiver processes bytes and frees buffer memory, it emits `WINDOW_UPDATE` frames to credit the sender with additional capacity.
- Prevents a slow stream consumer from choking the multiplexed transport of other streams.
