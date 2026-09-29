# Two-Phase Commit (2PC) Failure Modes

## The 2PC Protocol
1. Prepare Phase: Coordinator sends `PREPARE` to all participants. Participants lock local resources and respond `VOTE_COMMIT` or `VOTE_ABORT`.
2. Commit Phase: If all voted commit, coordinator sends `GLOBAL_COMMIT`. If any voted abort, coordinator sends `GLOBAL_ABORT`.

## The Blocking Problem
If the coordinator crashes after participants vote `VOTE_COMMIT` but before broadcasting `GLOBAL_COMMIT`, participants remain blocked indefinitely holding database locks. Three-Phase Commit (3PC) addresses this by introducing a non-blocking `PRE_COMMIT` phase.
