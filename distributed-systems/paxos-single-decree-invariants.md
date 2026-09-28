# Paxos: Single-Decree Invariants and Protocol Phases

## Roles
- Proposer: Proposes values.
- Acceptor: Votes on proposed values.
- Learner: Discovers chosen values.

## Phase 1 (Prepare / Promise)
1. Proposer chooses unique proposal number $n$ and sends `Prepare(n)` to a majority of Acceptors.
2. If $n >$ any proposal number previously promised by Acceptor, Acceptor replies `Promise(n, max_accepted_n, max_accepted_val)` and pledges never to accept any future proposals numbered $< n$.

## Phase 2 (Accept / Acknowledged)
1. If Proposer receives promises from a majority, it sends `Accept(n, v)` where $v$ is the value with highest proposal number among responses (or its own value if none reported).
2. Acceptor accepts `Accept(n, v)` unless it has promised a higher proposal number in the interim.
