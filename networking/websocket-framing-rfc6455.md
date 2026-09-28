# WebSocket Framing (RFC 6455)

## Wire Protocol Frame Structure
- `FIN` (1 bit): Indicates if this is the final fragment of a message.
- `Opcode` (4 bits): `0x1` (text), `0x2` (binary), `0x8` (close), `0x9` (ping), `0xA` (pong).
- `MASK` (1 bit): Must be set to 1 for all client-to-server frames.
- `Payload Length` (7, 7+16, or 7+64 bits).
- `Masking-Key` (32 bits, present only if MASK=1).

## Why Client Masking is Mandatory
Prevents Cache Poisoning attacks against intermediary transparent proxies. Without random masking, malicious web pages could construct payloads mimicking raw HTTP GET requests, tricking flawed proxies into caching poisoned responses.
