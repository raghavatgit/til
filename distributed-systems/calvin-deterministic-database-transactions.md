# Calvin: Deterministic Distributed Transaction Execution

The Calvin transaction management framework eliminates Distributed Two-Phase Commit (2PC) by ordering transactions deterministically before acquisition of locks.

## Core Layers
1. **Sequencer Layer**: Replicates transaction inputs into a global ordered log via Paxos.
2. **Scheduler Layer**: Examines ordered transaction inputs and deterministically allocates locks using static analysis.
3. **Execution Layer**: Executes transactions without lock wait deadlocks or abort rollbacks.
