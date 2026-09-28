# Google Spanner: TrueClock and External Consistency

## External Consistency (Strict Serializability)
If transaction $T_2$ begins after transaction $T_1$ commits in real time, $T_2$'s timestamp must be strictly greater than $T_1$'s timestamp:
$$t(T_2) > t(T_1)$$

## The TrueClock API
Hardware-synchronized clocks using atomic clocks (rubidium) and GPS receivers:
`TrueClock.now()` returns time interval $[t_{\text{earliest}}, t_{\text{latest}}]$ with bounded uncertainty $\epsilon \approx 1\text{ms}-7\text{ms}$.

## Commit Wait Rule
1. Transaction coordinator selects commit timestamp $s = t_{\text{latest}}$.
2. Coordinator delays releasing locks and responding to client until $t_{\text{earliest}} > s$.
Ensures timestamps match real-world causality without inter-datacenter lock communication.
