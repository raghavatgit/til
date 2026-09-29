# BGP EVPN and VXLAN Overlay Data Plane

BGP Ethernet VPN (EVPN) replaces flood-and-learn multicast with MP-BGP control plane route advertising for VXLAN overlays.

## Route Types
- **Type 2**: MAC/IP advertisement for endpoint discovery.
- **Type 3**: Inclusive Multicast Ethernet Tag for broadcast/multicast tree setup.
- **Type 5**: IP Prefix advertisement for inter-subnet L3 routing across data center fabrics.
