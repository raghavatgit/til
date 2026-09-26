# eXpress Data Path (XDP) Redirect & CPUMAP Scaling

## Bypassing SKB Allocation
Standard Linux packet receipt allocates a `sk_buff` kernel structure. XDP executes BPF programs directly in driver network ring buffers before `sk_buff` allocation.

---

## XDP Actions
* `XDP_DROP`: Discard at wire speed (DDoS mitigation).
* `XDP_TX`: Bounce packet back out the receiving interface.
* `XDP_REDIRECT`: Forward raw packet memory buffer to another interface via `DEVMAP` or distribute processing across target CPU cores via `CPUMAP`.
