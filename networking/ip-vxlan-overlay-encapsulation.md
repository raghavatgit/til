# VXLAN: Overlay Network Encapsulation (RFC 7348)

## Motivation
Traditional VLAN tags are 12 bits, limiting datacenters to 4,094 network segments. Modern cloud multitenancy requires hundreds of thousands of isolated tenant networks.

## Frame Encapsulation
VXLAN encapsulates Layer 2 Ethernet frames inside Layer 4 UDP datagrams:
- Outer IP Header + UDP Header (Port 4789).
- VXLAN Header: 24-bit Virtual Network Identifier (VNI), supporting over 16 million distinct isolated overlay networks.
- Inner Original Ethernet Frame.

## VTEP (VXLAN Tunnel Endpoint)
Hardware switches or software bridges (OVS, Linux kernel bridge) originate and terminate VXLAN tunnels, mapping tenant MAC addresses to physical underlay IP addresses.
