# WireGuard: Noise_IK Handshake and Cryptographic Session Rekeying

## Cryptographic Primitives
- Curve25519 (ECDH key exchange)
- ChaCha20-Poly1305 (Authenticated encryption)
- BLAKE2s (Hashing and HMAC)

## Handshake Patterns (Noise_IKpsk2)
- Initiator knows Responder's static public key ahead of time (`IK`).
- Handshake initiates with 1 round-trip message exchange:
  1. `Initiator -> Responder`: Ephemeral key, encrypted static key, encrypted timestamp.
  2. `Responder -> Initiator`: Ephemeral key, empty encrypted payload.
- Derive symmetric transport session keys.
- **Rekeying Invariant**: Sessions expire after 120 seconds or 2^64 messages; renegotiation occurs seamlessly without dropping packets.
