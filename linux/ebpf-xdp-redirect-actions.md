# eBPF XDP_REDIRECT and AF_XDP Zero-Copy

## Overview
`XDP_REDIRECT` enables the network interface driver to forward incoming raw packets to another network interface, a veth pair inside a container namespace, or an `AF_XDP` (XSK) user-space socket.

## Zero-Copy UMEM Rings
AF_XDP achieves zero-copy packet ingress through a user-allocated memory area (UMEM) partitioned into packet chunks:
- **Fill Ring**: User space deposits free buffer descriptors for the NIC driver to populate.
- **Rx Ring**: Driver deposits metadata pointers to received packets for user space to consume.
- **Tx Ring**: User space submits egress packet descriptors.
- **Completion Ring**: Driver notifies user space when Tx buffers can be safely reused.
