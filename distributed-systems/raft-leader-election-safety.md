# Raft Consensus: Leader Election Safety Invariant

## Core Invariant
If a leader has committed a log entry at a given index and term, no future leader for any term can contain a different entry at that index.

## Election Restriction Rule
A candidate node can only win an election if its log is at least as up-to-date as any other candidate in the majority quorum:
1. Compare Terms: Candidate with higher last log term wins.
2. Compare Lengths: If terms are equal, candidate with longer log wins.
