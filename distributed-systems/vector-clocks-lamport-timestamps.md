# Causality Tracking: Lamport Timestamps and Vector Clocks

## Lamport Timestamps
- Assigns a single scalar integer $L(e)$ to every event.
- Rule: Before an internal event, $L = L + 1$. When sending message, attach $L$. On receiving message with $L_{msg}$, update $L = \max(L, L_{msg}) + 1$.
- Limitation: If $L(a) < L(b)$, we CANNOT deduce that $a$ happened before $b$ (cannot detect concurrency).

## Vector Clocks
Maintains a vector $V$ of size $N$ (cluster node count):
- When node $i$ generates an event: $V_i[i] = V_i[i] + 1$.
- When node $i$ sends message, attaches $V_i$.
- When node $i$ receives $V_{msg}$: $V_i[j] = \max(V_i[j], V_{msg}[j])$ for all $j$, and $V_i[i] = V_i[i] + 1$.
- **Causal Rule**: $V_a < V_b \iff \forall k, V_a[k] \le V_b[k] \land \exists k, V_a[k] < V_b[k]$. If neither $V_a < V_b$ nor $V_b < V_a$, events are concurrent.
