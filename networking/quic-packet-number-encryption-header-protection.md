# QUIC Packet Number Encryption & Header Protection

## The Ossification Risk in TCP
Intermediate middleboxes (NATs, firewalls, DPI boxes) frequently inspect TCP sequence numbers and flags, preventing protocol evolution. RFC 9001 prevents middlebox ossification by encrypting QUIC packet numbers and headers.

---

## Cryptographic Mechanism
1. The packet payload is authenticated and encrypted using ChaCha20-Poly1305 or AES-GCM.
2. A sample of the encrypted ciphertext (typically 16 bytes) is passed to an AES-ECB / ChaCha20 header protection key.
3. The resulting mask XORs the packet number field and the least significant bits of the first header byte.
4. Passive observers cannot inspect packet sequences or infer loss rates.
