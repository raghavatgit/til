# BGP and Autonomous System Route Selection

## Overview
Border Gateway Protocol (BGP) is the de facto path-vector routing protocol of the global Internet, exchanging network reachability between Autonomous Systems (AS).

## Path Attributes & Best Path Selection Order
1. **Weight**: Cisco proprietary, local to router (highest wins).
2. **Local Preference**: AS-wide preference (highest wins).
3. **Locally Originated Route**: Routes generated locally via `network` or `aggregate`.
4. **AS Path Length**: Shortest number of autonomous systems traversed wins.
5. **Origin Code**: IGP < EGP < Incomplete.
6. **Multi-Exit Discriminator (MED)**: Lowest value preferred between neighbor AS.
7. **eBGP over iBGP**: External routes preferred over internal routes.
