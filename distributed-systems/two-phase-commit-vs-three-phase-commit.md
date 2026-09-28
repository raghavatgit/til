# Distributed Transactions: 2PC vs 3PC

## Two-Phase Commit (2PC)
1. **Prepare Phase**: Coordinator asks all participants: "Can you commit?". Participants write undo/redo logs and reply `VOTE_COMMIT` or `VOTE_ABORT`.
2. **Commit Phase**: If all vote commit, coordinator sends `GLOBAL_COMMIT`; if any abort, sends `GLOBAL_ABORT`.
- **Flaw**: 2PC is a blocking protocol. If the coordinator crashes after participants vote commit, participants must block indefinitely to preserve atomicity.

## Three-Phase Commit (3PC)
Splits commit phase into non-blocking stages:
1. `Can-Commit?`
2. `Pre-Commit` (coordinator acknowledges all voted yes; participants enter precommit).
3. `Do-Commit`
If coordinator crashes during 3PC, participants can safely timeout and determine state without deadlock.
