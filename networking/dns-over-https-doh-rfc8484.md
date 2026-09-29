# DNS-over-HTTPS (DoH, RFC 8484)

## Protocol Foundation
Encapsulates binary DNS wire format messages inside standard HTTP/2 and HTTP/3 POST or GET requests:
`GET /dns-query?dns=AAABAAABAAAAAAAAA3d3dwdleGFtcGxlA2NvbQAAAQAB HTTP/2`
`Accept: application/dns-message`

## Benefits
1. **Privacy**: End-to-end encryption eliminates ISP snooping and DNS hijacking on public Wi-Fi.
2. **CDN Performance**: HTTP/2 multiplexing allows combining DNS resolution and asset fetching on the same origin connection.
3. **HTTP Caching**: Resolvers can set `Cache-Control: max-age=N` matching DNS record TTLs, enabling standard web reverse proxies to cache DNS responses.
