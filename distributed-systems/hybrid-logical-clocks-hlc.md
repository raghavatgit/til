# Hybrid Logical Clocks (HLC)

## Problem with Pure Physical vs Logical Clocks
- Physical clocks (NTP) suffer clock skew and non-monotonic backward jumps.
- Logical clocks (Lamport) track causality but diverge from real-world wall-clock time.

## HLC State: $(l, c)$
- $l$: Physical component (tracks highest known physical time).
- $c$: Logical counter (increments during events occurring within the same physical tick).

## Update Rules
On local event:
$$l' = \max(l, \text{pt}_{\text{now}}), \quad c' = (l' == l) ? c + 1 : 0$$
Guarantees causality ($e_1 \to e_2 \implies \text{hlc}(e_1) < \text{hlc}(e_2)$) while bounding physical time divergence: $|l - \text{pt}| \le \epsilon$.
