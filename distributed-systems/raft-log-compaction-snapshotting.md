# Raft Consensus: Log Compaction and Snapshotting

## The State Growth Challenge
In the Raft consensus algorithm, the replicated log continuously grows as client requests arrive. In long-running systems, storing the full log leads to two critical problems:
1. Boundless disk memory consumption.
2. Prohibitive node reboot recovery times (replaying millions of state transitions).

## The Snapshotting Architecture
Log compaction truncates the log by replacing committed entries with a point-in-time state machine snapshot:
- Each server takes snapshots independently, covering only committed entries up to `lastIncludedIndex` and `lastIncludedTerm`.
- Once the state machine serializes its state to persistent storage, the Raft log discards entries up to `lastIncludedIndex`.
- The snapshot metadata retains:
  - `lastIncludedIndex`: The latest log index covered by the snapshot.
  - `lastIncludedTerm`: The term of this latest log entry.
  - Configuration state: Latest cluster membership specification.

## InstallSnapshot RPC
When a follower falls significantly behind (e.g. following network partition or node replacement) such that the leader has already discarded log entries needed by the follower, normal `AppendEntries` cannot catch it up.

The leader initiates the `InstallSnapshot` RPC:
```
Arguments:
  term: leader's current term
  leaderId: so follower can redirect clients
  lastIncludedIndex: snapshot replaces all entries up through this index
  lastIncludedTerm: term of lastIncludedIndex
  offset: byte offset where chunk is positioned in the snapshot file
  data[]: raw bytes of the snapshot chunk
  done: true if this is the last chunk
```

Upon receiving a valid `InstallSnapshot`:
1. Follower resets state machine using snapshot bytes.
2. Discards any log entry with index `<= lastIncludedIndex`.
3. If an existing log entry has the same index and term as `lastIncludedIndex`, retain subsequent log entries and discard the rest.
4. State machine invariants are preserved with bounded storage footprint.
