# Consensus: Classic Paxos vs Multi-Paxos

## Classic Paxos (Per Instance)
- **Phase 1a (Prepare)**: Proposer chooses proposal number $n$ and sends `Prepare(n)` to acceptors.
- **Phase 1b (Promise)**: Acceptors promise not to accept proposals $< n$ and return highest-numbered proposal accepted so far.
- **Phase 2a (Accept)**: Proposer sends `Accept(n, value)` to quorum.
- **Phase 2b (Accepted)**: Acceptors accept and notify learners.
Requires 2 full network round-trips for every single decision.

## Multi-Paxos Optimization
Amortizes Phase 1 across a stream of log decisions:
Once a leader successfully runs Phase 1 for all subsequent log positions, it only executes Phase 2 (`Accept`/`Accepted`) for each successive log entry, halving commit latency to 1 RTT.
