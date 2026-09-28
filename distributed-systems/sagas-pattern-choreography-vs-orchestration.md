# Saga Pattern: Distributed Long-Running Transactions

## Overview
Traditional ACID 2PC does not scale across microservices boundaries. The Saga pattern breaks a distributed transaction into a sequence of local transactions:
$$T_1, T_2, \dots, T_n$$
Each transaction updates data within a single service. If step $T_i$ fails, compensating transactions execute in reverse order:
$$C_{i-1}, C_{i-2}, \dots, C_1$$

## Choreography vs Orchestration
- **Choreography**: Services publish events (e.g., via Kafka) and subscribe to partner events. Decentralized but difficult to trace and prone to cyclic dependencies.
- **Orchestration**: A centralized orchestrator executes a state machine, sending explicit command messages to participant services and coordinating compensations upon failure.
