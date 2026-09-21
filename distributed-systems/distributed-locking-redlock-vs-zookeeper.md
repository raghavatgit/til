# Distributed Systems: Redlock vs ZooKeeper Distributed Locks (Kleppmann Critique)

## The Redlock Algorithm
Proposed by Salvatore Sanfilippo:
- Client acquires lock across $N$ independent Redis nodes by writing key with random value and TTL.
- Lock acquired if client sets key on $\ge \lfloor N / 2 \rfloor + 1$ nodes before validity timeout expires.

## The Martin Kleppmann Critique (2016)
Redlock relies on asynchronous physical clocks (synchrony assumption):
1. **Process Pauses (GC/Stop-the-World)**:
   - Client A acquires Redlock.
   - Client A pauses in a 15-second Java GC pause or VM migration.
   - Lock TTL expires.
   - Client B acquires lock on Redis nodes.
   - Client A resumes and writes to storage concurrently with Client B, corrupting shared data.
2. **Clock Drift**:
   - NTP clock jumps on Redis nodes can expire locks prematurely.

## The Correct Solution: Fencing Tokens with ZooKeeper / Raft
1. Lock manager must generate a strictly monotonically increasing **fencing token** (e.g. `zxid` or counter) with each lock grant.
2. Storage service verifies fencing token: if incoming write token $\le$ last processed token, reject write.
3. Completely neutralizes GC pauses and out-of-order write hazards.
