# WireGuard Cryptographic Routing and Mesh Networks

## WireGuard Core Mechanics
- Replaces complex IPSec/OpenVPN state machines with simple Cryptokey Routing.
- Maps public encryption keys directly to allowed IP address ranges (`AllowedIPs`).
- State-less from user perspective: roaming devices update endpoints automatically via Noise protocol handshakes.

## Coordination via DERP (Designated Encrypted Relay for Packets)
- In full-mesh architectures (Tailscale), nodes communicate point-to-point via STUN/UDP hole punching.
- If direct UDP is blocked by firewall/symmetric NAT, traffic automatically falls back to encrypted HTTP/2 relays (DERP nodes) without breaking connection state.
