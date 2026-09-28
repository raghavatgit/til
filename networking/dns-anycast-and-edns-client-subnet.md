# DNS Anycast Routing and EDNS Client Subnet (ECS)

## Anycast Routing
- Multiple physical DNS servers across global datacenters advertise the exact same IP address via BGP.
- Internet routers use BGP metrics (AS-Path length) to route user queries to the topologically nearest node.
- Provides automatic DDoS mitigation and instantaneous geographic failover.

## EDNS Client Subnet (ECS, RFC 7871)
When recursive resolvers query authoritative DNS, Anycast routes to the resolver's closest edge, which may be geographically distant from the end user.
ECS appends the user's subnet (e.g., `/24` for IPv4) to the DNS request, allowing authoritatives to return geolocation-optimized CDN endpoints.
