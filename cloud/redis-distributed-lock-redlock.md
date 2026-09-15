# Distributed Locking Invariants: Redlock, Clock Drift, and Fencing Tokens

*Date: 2026-09-15*  
*Category: Cloud & Backend / Distributed Systems*

## Overview

In distributed architectures, ensuring mutual exclusion across distributed microservices requires distributed locks. The naive approach (`SET resource_name my_random_value NX PX 30000`) works on a single Redis instance, but fails during master failover due to asynchronous replication.

## Redlock Algorithm (Multi-Master Consensus)

To acquire a lock across `N` independent Redis masters (typically 5):
1. Capture current timestamp $T_1$.
2. Attempt to acquire lock in all $N$ instances sequentially using identical key and random value with short timeout.
3. Compute elapsed time: $\Delta t = T_2 - T_1$.
4. Lock is acquired if and only if acquired from at least $\lfloor N/2 \rfloor + 1$ instances and $\Delta t < \text{TTL}$.
5. Validity time remaining: $\text{TTL} - \Delta t - \text{ClockDriftMargin}$.

## The Martin Kleppmann Critique: Process Pauses

Even with Redlock, long GC pauses, page faults, or network blips can cause a client's lease to expire *without the client knowing it*. The client awakens and writes stale data to shared storage, violating mutual exclusion.

## The Solution: Monotonic Fencing Tokens

To guarantee absolute safety, storage systems must enforce monotonically increasing fencing tokens:

```text
[ Client 1 ] -- Acquired Lock (Token 33) --> [ Storage: Write OK (Token 33) ]
     |
  [ GC Pause: Lock expires, Client 2 acquires Token 34 ]
     |
[ Client 2 ] -----------------------------> [ Storage: Write OK (Token 34) ]
     |
[ Client 1 Awakens ] -- Attempt Write (Token 33) --> [ Storage: REJECT (33 < 34) ]
```

## Key Takeaway
A distributed lock algorithm alone cannot guarantee safety against process pauses unless the target storage resource enforces monotonic fencing token checks.
