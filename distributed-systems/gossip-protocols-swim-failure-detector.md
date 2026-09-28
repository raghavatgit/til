# SWIM Gossip Protocol: Scalable Weakly-Consistent Infection-Style Membership

## Traditional Heartbeat Limitations
Heartbeating to a central registry or all-to-all broadcasting scales at O(N^2) network messages, failing at thousands of cluster nodes.

## SWIM Protocol Mechanics
1. Every period $T$, node $A$ randomly selects node $B$ and sends `ping`.
2. If $B$ acknowledges with `ack` within timeout, $B$ is marked healthy.
3. If no ack, $A$ selects $k$ random indirect nodes and sends `ping-req(B)`. Those nodes attempt to ping $B$ directly.
4. If indirect pings fail, $B$ is placed in `Suspect` state.
5. If no refutation message arrives during a grace period, $B$ is marked `Dead` and gossip propagates the state across the cluster in O(log N) time.
