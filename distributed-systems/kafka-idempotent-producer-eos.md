# Apache Kafka: Idempotent Producers and Exactly-Once Semantics

## Idempotent Producer Invariants
- Each producer is assigned a 64-bit Producer ID (PID) via `InitProducerId`.
- Each partition message appends a monotonically increasing Sequence Number.
- Broker deduplication: If a broker receives sequence number $\le$ last committed sequence number, it acknowledges without writing a duplicate record.

## Transactional Coordinator
1. Producer registers partitions with Transaction Coordinator.
2. Writes data to partitions.
3. Writes commit marker to `__transaction_state` topic.
4. Coordinator writes commit marker to consumer partitions; consumer only reads messages committed in two-phase protocol (`read_committed`).
