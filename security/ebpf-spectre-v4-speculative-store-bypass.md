# eBPF Spectre v4: Speculative Store Bypass (SSB)

## Transient Execution Attacks in Kernel eBPF
Speculative execution CPUs may execute memory reads before prior store instructions have committed their addresses, temporarily loading stale or out-of-bounds memory data during speculative windows.

---

## Kernel Verifier Mitigations
The Linux eBPF verifier analyzes BPF bytecode and inserts memory barrier instructions (`speculative_store_bypass_disable` or conditional masking) to serialize memory loads following dynamic stack writes, preventing leak of kernel memory to userland.
