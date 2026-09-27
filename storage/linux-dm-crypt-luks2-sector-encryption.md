# Linux dm-crypt & LUKS2 Sector Encryption

## Device Mapper Architecture
`dm-crypt` is a Linux kernel block device driver that provides transparent sector-by-sector encryption using the Kernel Crypto API (typically AES-XTS-PLAIN64).

---

## LUKS2 Enhancements
* **Argon2id KDF:** Resists GPU and ASIC brute-force attacks on passphrases.
* **Resilience Headers:** Dual redundant metadata headers prevent disk corruption lockouts.
* **Integrity Protection:** Optional dm-integrity verification using HMAC-SHA256 catches physical bit-rot or offline tampering.
