# DNS-over-HTTPS (DoH) vs DNS-over-TLS (DoT)

## Privacy Against ISP Snooping
Plaintext DNS (port 53 UDP) is unencrypted and subject to man-in-the-middle snooping, hijacking, and ISP tampering.

---

## Architectural Comparison
* **DNS-over-TLS (DoT, RFC 7858):**
  - Encapsulates standard DNS packets inside a dedicated TLS connection over TCP port 853.
  - Dedicated port allows network administrators to easily identify and monitor or filter DNS traffic.
* **DNS-over-HTTPS (DoH, RFC 8484):**
  - Encapsulates DNS wireformat inside standard HTTP/2 or HTTP/3 binary requests over TCP/UDP port 443.
  - Blends in indistinguishably with regular HTTPS web traffic, preventing port-based blocking.
