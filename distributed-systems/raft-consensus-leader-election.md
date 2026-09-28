# Raft Consensus: Leader Election Mechanics

## State Machine Model
Each node in a Raft cluster operates in one of three states:
- **Leader**: Handles all client requests, replicates log entries.
- **Follower**: Passive, responds to incoming RPCs from leaders and candidates.
- **Candidate**: Transitions here upon heartbeat timeout to initiate election.

## Election Protocol
1. If a follower receives no heartbeat within its randomized election timeout (150ms - 300ms), it increments `currentTerm`, transitions to Candidate, votes for itself, and broadcasts `RequestVote` RPCs.
2. **Election Safety Rule**: A candidate is granted a vote only if its log is at least as up-to-date as the voter's log (comparing `(lastLogTerm, lastLogIndex)`).
3. First candidate to receive votes from a strict majority ($N/2 + 1$) becomes Leader and immediately broadcasts heartbeats.
