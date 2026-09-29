# eBPF Sockops: Socket-Level Redirection

## The Local Loopback Problem
When two microservices on the same host communicate over localhost or unix domain sockets, packets traverse the full TCP/IP networking stack (IP lookup, routing, netfilter/iptables, TCP state machine), incurring unnecessary latency.

## Sockops and Sockhash Acceleration
1. Attach an eBPF `sock_ops` program to the root cgroup.
2. When a TCP connection is established on localhost, the BPF program registers the socket file descriptors into a `BPF_MAP_TYPE_SOCKHASH` indexed by the 4-tuple.
3. When data is sent, `bpf_msg_redirect_hash()` intercepts the payload directly at the socket send buffer and transfers it immediately to the peer socket's receive queue, completely bypassing the TCP/IP network stack.
