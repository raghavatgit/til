# BGP Best Path Selection Algorithm

## Autonomous System Routing Hierarchy
Border Gateway Protocol (BGP, RFC 4271) evaluates competing route advertisements across internet Autonomous Systems (AS) using a deterministic multi-step decision tiebreaker:

---

## Attribute Evaluation Order (Tiebreakers)
1. **Highest Weight:** Local to the router (Cisco proprietary).
2. **Highest LOCAL_PREF:** Preferred path within the local AS (default 100).
3. **Locally Originated Routes:** Routes originated via `network` or `aggregate-address`.
4. **Shortest AS_PATH:** Route traversing the fewest AS numbers.
5. **Lowest Origin Type:** IGP < EGP < INCOMPLETE.
6. **Lowest MED (Multi-Exit Discriminator):** Informs neighboring AS of preferred ingress link into local AS.
7. **eBGP over iBGP:** Exterior peers favored over internal peers.
8. **Lowest IGP Metric to BGP Next-Hop:** Shortest internal path.
9. **Lowest Router ID / Neighbor IP:** Final tiebreaker.
