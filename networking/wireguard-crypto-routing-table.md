# WireGuard Cryptokey Routing Semantics

WireGuard structures peer associations through a Cryptokey Routing Table binding public keys directly to permitted IP ranges.

## Lookup Rules
1. Outbound: Destination IP is evaluated against `AllowedIPs` trie to select target peer public key.
2. Inbound: Decrypted packet inner IP is checked against sender peer `AllowedIPs`. Packets with non-matching source IPs are dropped silently.
