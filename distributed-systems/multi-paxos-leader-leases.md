# Multi-Paxos: Steady-State Optimization and Leader Leases

## Phase 1 Elimination
In Multi-Paxos, once a leader successfully completes Phase 1 (Prepare/Promise) across a majority of Acceptors for an initial proposal, it can reuse the same proposal number $n$ for all subsequent log instances:
- Eliminates 1 RTT overhead: steady-state commits execute Phase 2 (Accept/Ack) in just 1 RTT.

## Leader Leases
- Master leases eliminate quorum read overhead.
- Acceptors grant leader a time-bounded lease during which they promise never to vote for a different proposer.
- The leader serves read-only queries locally without network roundtrips, bounded by clock drift guarantees.
