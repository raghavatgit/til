# NAT Traversal: STUN, TURN, and ICE

## NAT Types
- Full Cone NAT
- Restricted Cone NAT
- Port-Restricted Cone NAT
- Symmetric NAT (allocates a new external port for every distinct remote destination IP/port).

## Protocols
1. **STUN (Session Traversal Utilities for NAT)**: Client queries STUN server to discover its public reflexive IP and port mapping. Fails on Symmetric NAT.
2. **TURN (Traversal Using Relays around NAT)**: Relays all media/packets through a central server when direct peer-to-peer punching fails. High bandwidth cost.
3. **ICE (Interactive Connectivity Establishment)**: Systematic algorithm that gathers candidates (Host, Server Reflexive, Relay) and tests connectivity pairs in order of priority.
