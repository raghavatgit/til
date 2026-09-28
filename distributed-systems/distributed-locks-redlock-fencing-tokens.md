# Distributed Locks: Redlock vs Fencing Tokens

## The Redlock Algorithm
Designed for Redis clusters:
1. Client acquires current timestamp $t_1$.
2. Attempts to acquire lock across $N$ independent Redis instances sequentially using `SET resource_name my_random_value NX PX 30000`.
3. If acquired on majority ($N/2 + 1$) within total time $< \text{validity\_time}$, lock is held.

## The Martin Kleppmann Critique
In asynchronous systems with GC pauses or network delays:
1. Client 1 acquires lock.
2. Client 1 suffers a stop-the-world garbage collection pause.
3. Lock lease expires on storage nodes.
4. Client 2 acquires lock and updates shared resource.
5. Client 1 awakens and executes write, corrupting data.

## Solution: Fencing Tokens
Storage engines must mandate monotonically increasing fencing tokens: Any write with a token $< \text{highest\_seen\_token}$ is rejected.
