# Raft: Log Replication and State Safety Invariants

## Log Entry Structure
Each log entry contains an index, creation term, and client state-machine command.

## Log Matching Property
1. If two entries in different logs have the same index and term, they store the same command.
2. If two entries in different logs have the same index and term, their logs are identical in all preceding entries.

## Leader Completeness Invariant
If a log entry is committed in a given term, that entry will be present in the logs of the leaders for all higher-numbered terms. A leader never overwrites or truncates its own log; it only appends entries.
