# CAP Theorem and PACELC Theorem

## CAP Theorem (Brewer)
In a network subject to Partitions (P), a distributed system can guarantee at most:
- **Consistency (C)**: Every read receives the most recent write or an error.
- **Availability (A)**: Every non-failing node returns a non-error response without guarantee of latest write.

## PACELC Theorem (Abadi)
CAP only describes behavior during rare network partitions. PACELC extends this to normal operations:
- If there is a **Partition (P)**: How does the system choose between **Availability (A)** and **Consistency (C)**?
- **Else (E)**: How does the system choose between **Latency (L)** and **Consistency (C)**?

Examples:
- DynamoDB / Cassandra: PA/EL (favors availability during partition, low latency during normal operation).
- Spanner / CockroachDB: PC/EC (favors consistency during partition, consistency over latency normally).
