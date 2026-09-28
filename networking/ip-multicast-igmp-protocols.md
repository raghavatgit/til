# IP Multicast and IGMP Protocol Architecture

## Addressing
- IPv4 Class D addresses: `224.0.0.0` to `239.255.255.255`.
- Mapped to Ethernet multicast MAC addresses using OUI `01:00:5E` with the lower 23 bits of the IP address placed into the MAC.

## IGMP (Internet Group Management Protocol)
- Hosts use IGMP to report multicast group membership to local multicast routers.
- **IGMP Snooping**: Layer 2 switches listen to IGMP Join/Leave messages to constrain multicast packet replication only to switch ports with subscribed members, preventing network flooding.
