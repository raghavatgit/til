# Raft Dynamic Cluster Membership: Joint Consensus

## The Split-Brain Risk in Membership Reconfiguration
Naively switching a Raft cluster from 3 nodes to 5 nodes can produce two disjoint majorities if nodes switch configurations at different times, violating safety.

---

## Joint Consensus Two-Phase Transition
Diego Ongaro's Raft protocol solves this with Joint Consensus:
1. **Phase 1 (Joint Configuration `C_old,new`):**
   - The leader appends entry `C_old,new`.
   - Log commits require separate majorities from *both* `C_old` configuration and `C_new` configuration.
2. **Phase 2 (Final Configuration `C_new`):**
   - Once `C_old,new` is committed, leader commits `C_new`.
   - Cluster safely transitions to the new membership without downtime or split-brain risk.
