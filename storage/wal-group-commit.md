# Write-Ahead Log (WAL) Group Commit Optimization

## The fsync() Bottleneck
Each database transaction commit requires calling `fsync()` on the WAL to guarantee Durability (ACID). Standard SSDs handle at most 10,000 - 50,000 fsyncs per second.

## Group Commit Algorithm
Instead of executing an `fsync()` per transaction:
1. The first committing thread becomes the Group Leader.
2. Concurrent committing threads append their log records into the WAL buffer and register as followers.
3. The Group Leader issues a single `fsync()` that flushes all batched records simultaneously, then wakes up all followers.
