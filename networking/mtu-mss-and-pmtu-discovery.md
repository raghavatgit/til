# MTU, MSS, and Path MTU Discovery (PMTUD)

## Key Concepts
- **MTU (Maximum Transmission Unit)**: Maximum size of an IP packet that can be transmitted on a link without fragmentation (standard Ethernet: 1500 bytes).
- **MSS (Maximum Segment Size)**: Maximum payload of a TCP segment: `MSS = MTU - (IP Header + TCP Header) = 1500 - 40 = 1460 bytes`.

## Path MTU Discovery (PMTUD)
- TCP sets the Don't Fragment (DF) bit in IP headers.
- If an intermediate router has a smaller MTU, it drops the packet and responds with ICMP Type 3 Code 4 ("Fragmentation Needed and DF set"), specifying the next-hop MTU.
- **ICMP Blackhole**: Misconfigured firewalls that drop ICMP break PMTUD, causing connections to hang on large transfers. Fixed via TCP MSS clamping (`iptables -t mangle -A POSTROUTING -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu`).
