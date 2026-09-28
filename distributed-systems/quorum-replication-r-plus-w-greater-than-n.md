# Quorum Replication: R + W > N

## Parameters
- $N$: Number of replicas in the replica set.
- $W$: Write quorum (number of replicas that must acknowledge a write).
- $R$: Read quorum (number of replicas that must be queried during a read).

## Strict Consistency Invariant
$$R + W > N$$
By Pigeonhole Principle, any read quorum of size $R$ and write quorum of size $W$ must overlap in at least one replica. This overlapping replica holds the latest version of the data, identified via version timestamps.

## Common Configurations
- $N=3, W=2, R=2$: Tolerates 1 replica failure for both reads and writes.
- $N=5, W=3, R=3$: Tolerates 2 replica failures.
