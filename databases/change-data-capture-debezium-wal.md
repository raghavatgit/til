# Change Data Capture (CDC): WAL Log Parsing

## The Dual-Write Problem
Updating a database and writing an event to Apache Kafka sequentially in application code creates split-brain state if either operation fails or crashes midway.

## CDC Architecture
CDC tools (such as Debezium) read changes directly from the database's internal transaction log:
- PostgreSQL: Logical Decoding output plugins (`pgoutput`).
- MySQL: Row-based Binary Log (`binlog`).

## Guarantees
- Zero application dual-writes.
- Exactly ordered change stream reflecting committed transaction history.
- Extremely low latency (milliseconds from DB commit to Kafka message publication).
