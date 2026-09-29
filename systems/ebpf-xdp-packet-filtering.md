# eBPF and XDP: High-Performance Kernel Bypass Architecture

## Overview
eXpress Data Path (XDP) provides a bare-metal packet processing framework directly inside the Linux network driver layer, executing before the kernel allocates an `sk_buff` (socket buffer) or triggers TCP/IP stack overhead.

## Execution Path Comparison
1. Standard Linux Path: NIC -> DMA -> Ring Buffer -> Hard IRQ -> NAPI poll -> `sk_buff` alloc -> Netfilter / iptables -> Socket Queue.
2. XDP Execution Path: NIC -> DMA -> XDP Program Execution (L1 CPU Cache) -> Immediate Action (`XDP_DROP`, `XDP_TX`, `XDP_REDIRECT`, `XDP_PASS`).

## Core Invariants
- Zero Allocation: Packets are processed in-place via direct memory pointers `ctx->data` and `ctx->data_end`.
- Safety Verification: The eBPF static bytecode verifier enforces memory boundary proofs prior to kernel loading.
