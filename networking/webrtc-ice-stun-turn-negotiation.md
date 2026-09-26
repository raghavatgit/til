# WebRTC ICE Candidate Gathering & NAT Traversal

## Overview: Peer-to-Peer Connectivity Behind NATs
Direct peer-to-peer UDP communication across the internet is complicated by Network Address Translation (NAT) and stateful firewalls. WebRTC resolves this via the Interactive Connectivity Establishment (ICE, RFC 8445) protocol combining STUN and TURN.

---

## ICE Candidate Hierarchy

During call setup, peers gather three distinct candidate types:

1. **Host Candidates (`host`)**:
   - Local IP addresses bound directly to system network interfaces (e.g. `192.168.1.50:54321` or link-local IPv6).
   - Direct connection possible if both peers reside on the same LAN/VPC.

2. **Server Reflexive Candidates (`srflx`)**:
   - Public IP and port discovered by querying a Session Traversal Utilities for NAT (STUN, RFC 5389) server.
   - The STUN server reflects back the external source IP and mapped port created by the peer's NAT router.
   - Enables direct peer connection across Cone NATs and Address-Restricted NATs.

3. **Relay Candidates (`relay`)**:
   - Traversal Using Relays around NAT (TURN, RFC 5766) allocation.
   - Required when one or both peers are behind Symmetric NATs (where destination IP alters port mappings).
   - Traffic routes through the TURN server as an encrypted media proxy.

---

## Trickle ICE Protocol Flow

Instead of blocking SDP exchange until all candidates are gathered (which adds 2-5 seconds of call setup latency), modern WebRTC implementations use **Trickle ICE**:

```
Peer A                                Signaling Server                             Peer B
  |                                          |                                        |
  |--- Create Offer (SDP without candidates)->|--------------------------------------->|
  |                                          |<-- Create Answer (SDP) ----------------|
  |<-- Answer delivered ---------------------|                                        |
  |                                          |                                        |
  |-- onicecandidate (host) ---------------->|--- candidate:host -------------------->|
  |-- onicecandidate (srflx) --------------->|--- candidate:srflx ------------------->|
  |-- onicecandidate (relay) --------------->|--- candidate:relay ------------------->|
  |                                          |                                        |
  |<=========== ICE Connectivity Checks (STUN Binding Requests via UDP) =============>|
  |<=========== Nominated Pair Established: DTLS / SRTP Media Flow ==================>|
```
