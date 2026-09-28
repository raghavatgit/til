# TLS 1.3: 1-RTT Handshake and Forward Secrecy

## TLS 1.2 vs TLS 1.3
- TLS 1.2 required 2 round-trip times (2-RTT) to negotiate cipher suites and establish session keys.
- TLS 1.3 reduces standard handshake to 1-RTT (and 0-RTT for resumed sessions).

## Handshake Flow
1. **ClientHello**: Sends supported ciphers AND speculative ECDH key share (`KeyShareEntry`).
2. **ServerHello**: Selects cipher, sends server ECDH key share, certificates, and handshake MAC.
3. Both sides independently derive symmetric keys (`traffic_secret_0`) and begin encrypted application data exchange immediately.

## Forward Secrecy (FS)
Eliminated static RSA key exchange. All sessions mandate Ephemeral Diffie-Hellman (ECDHE); compromise of private server keys cannot decrypt historical session captures.
