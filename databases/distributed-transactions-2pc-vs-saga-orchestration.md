# Distributed Transactions: Two-Phase Commit (2PC) vs Saga Pattern

## The Blocking Vulnerability in 2PC
Two-Phase Commit (Prepare -> Commit) ensures strict ACID serializability across distributed databases. However, 2PC is a blocking protocol: if the Coordinator crashes after Phase 1, participants must hold row locks indefinitely, starving concurrent traffic.

---

## The Saga Pattern: Eventual Consistency
Sagas replace distributed locking with a sequence of local transactions:
* Each service executes its local transaction and publishes an event.
* If a step fails, the Saga orchestrator triggers compensating transactions (rollback actions) in reverse order.
* Trades ACID isolation for high availability and resilient partition tolerance.
