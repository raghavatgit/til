# Atomic Commitment: Two-Phase Commit (2PC)

## Protocol Steps
1. **Phase 1 (Prepare)**:
   - Coordinator sends `PREPARE` to all cohort participants.
   - Participants write decision to local log and respond `VOTE_COMMIT` or `VOTE_ABORT`.
2. **Phase 2 (Commit/Abort)**:
   - If all voted commit: Coordinator logs `COMMIT` and sends `GLOBAL_COMMIT`.
   - If any voted abort: Coordinator logs `ABORT` and sends `GLOBAL_ABORT`.

## The Blocking Vulnerability
2PC is an inherently blocking protocol:
If coordinator crashes after cohorts have voted `VOTE_COMMIT` but before broadcasting `GLOBAL_COMMIT`, cohorts must wait indefinitely because they cannot autonomously decide whether other cohorts committed or aborted.
