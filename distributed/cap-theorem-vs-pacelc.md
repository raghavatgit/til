# Distributed Systems: CAP Theorem vs PACELC

## CAP Theorem Formulation
In an asynchronous network subject to partitions ($P$):
A distributed system can guarantee at most two of:
- **Consistency ($C$)**: Every read receives the most recent write or an error.
- **Availability ($A$)**: Every non-failing node returns a non-error response.
- **Partition Tolerance ($P$)**: The system continues to operate despite dropped or delayed network messages.

## PACELC Theorem (Abadi)
CAP only considers behavior *during* an active network partition ($P$).
PACELC addresses the broader reality:
- **If Partition ($P$)**: Choose between Availability ($A$) and Consistency ($C$).
- **Else ($E$)**: Choose between Latency ($L$) and Consistency ($C$).

Examples:
- DynamoDB / Cassandra: PA/EL (Partition Available, Else Latency).
- Bigtable / Spanner: PC/EC (Partition Consistent, Else Consistent).
