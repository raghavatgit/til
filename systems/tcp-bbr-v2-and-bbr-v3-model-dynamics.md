# TCP BBRv2 & BBRv3 Congestion Control Dynamics

## The Pitfalls of Loss-Based Congestion Control
Traditional algorithms (Reno, CUBIC) treat packet drops as the primary indicator of congestion. On modern networks with large router buffers (Bufferbloat), loss-based algorithms inflate queuing delay and round-trip times (RTT) by orders of magnitude before dropping packets.

---

## BBR Core Operating Philosophy
BBR (Bottleneck Bandwidth and Round-trip propagation time) models the physical communication pipe using two parameters:
- `BtlBw`: Estimated bottleneck link bandwidth.
- `RTprop`: Estimated two-way propagation delay.

The target inflight data volume is governed by the Bandwidth-Delay Product (BDP):
$$\text{BDP} = \text{BtlBw} \times \text{RTprop}$$

---

## Advancements in BBRv2 & BBRv3
1. **ECN (Explicit Congestion Notification) Coexistence**:
   - BBRv1 ignored packet loss and ECN marks during bandwidth probing, causing unfairness to concurrent CUBIC streams.
   - BBRv2/v3 dynamically caps inflight pacing rate when DCTCP/L4S ECN marking thresholds (`ceil(alpha * inflight)`) are triggered.

2. **Loss Response**:
   - Moderates aggressive bandwidth probing upon detecting non-transient packet drops.

3. **Rapid Convergence**:
   - Faster cycle drain intervals to resolve network buffer bloat and achieve fair bandwidth distribution among heterogeneous flows.
