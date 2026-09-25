# Distributed Systems: SWIM Gossip Protocol

## Problem with Heartbeating
Traditional centralized heartbeating incurs $O(N)$ network messages per node per period, resulting in $O(N^2)$ message storms as cluster membership scales to thousands of servers.

## SWIM (Structured Weakly-Consistent Infection-Style Membership)
1. **Failure Detection**:
   - Node $A$ sends `ping` to random target node $B$.
   - If no `ack` received within timeout: $A$ requests $k$ random members to send indirect pings to $B$.
   - Prevents false positives caused by local network route degradation.
2. **Suspicion Mechanism**:
   - Rather than instantly declaring a non-responsive node dead, mark it as `Suspect` for period $T_{suspect}$.
   - The suspect node can refute the accusation if alive.
3. **Dissemination**:
   - Membership changes are piggybacked onto regular ping/ack messages, spreading epidemically in $O(\log N)$ time.
