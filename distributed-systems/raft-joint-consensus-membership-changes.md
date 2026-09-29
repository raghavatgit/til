# Raft: Joint Consensus Configuration Changes

## The Single-Server Change Limitation
Switching cluster configuration directly from $C_{\text{old}}$ to $C_{\text{new}}$ is unsafe: depending on network timing, two disjoint majorities could elect separate leaders concurrently.

## Joint Consensus Algorithm
Configuration changes transition through an intermediate joint state:
$$C_{\text{old,new}}$$
1. Leader writes and commits $C_{\text{old,new}}$ configuration entry.
2. In $C_{\text{old,new}}$, decisions (elections, log commits) mandate separate majorities from **both** $C_{\text{old}}$ and $C_{\text{new}}$.
3. Once $C_{\text{old,new}}$ is committed, Leader writes and commits $C_{\text{new}}$, safely demoting removed nodes.
