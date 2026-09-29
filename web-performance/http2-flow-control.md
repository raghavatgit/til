# HTTP/2 Flow Control Windows

## Architecture
HTTP/2 implements credit-based flow control on both individual streams and the overall connection via `WINDOW_UPDATE` frames.

## Starvation Prevention
If an endpoint reads data slower than the network delivers, the stream flow control window shrinks to zero, stopping transmission on that stream without blocking high-priority control frames or other parallel streams.
