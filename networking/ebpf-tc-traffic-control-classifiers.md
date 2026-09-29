# eBPF Traffic Control (tc) Classifiers and Shaping

`clsact` qdisc attaches eBPF programs to kernel network layers before IP routing and socket allocation.

## Comparison with XDP
- XDP executes before `sk_buff` allocation (fastest, L2/L3 focus).
- TC executes on `struct __sk_buff` (supports L4-L7 inspection, packet redirection, encapsulation, egress shaping).

## Return Codes
- `TC_ACT_OK`: Pass packet through standard networking stack.
- `TC_ACT_SHOT`: Drop packet immediately.
- `TC_ACT_REDIRECT`: Reroute to alternative interface or network namespace.
