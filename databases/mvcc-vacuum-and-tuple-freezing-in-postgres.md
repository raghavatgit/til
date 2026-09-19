# Databases: PostgreSQL MVCC, Dead Tuples, and 32-Bit Wraparound Freezing

## The Multi-Version Concurrency Control Model
In PostgreSQL:
- An `UPDATE` does not overwrite the row on disk; it marks the existing row version (`xmin`/`xmax`) as dead and inserts a new physical tuple.
- A `DELETE` marks the tuple dead by populating `xmax` with the deleting transaction's ID.
- Dead tuples remain on disk until reclaimed by `VACUUM`.

## Dead Tuple Bloat and HOT Updates
- If a table undergoes heavy writes without index modification, Heap-Only Tuple (HOT) updates allow new tuples on the same 8KB disk page to chain without updating index entries.
- If pages lack space, new heap pages are allocated, causing table and index bloat.

## The 32-Bit Transaction ID Wraparound Threat
PostgreSQL transaction IDs (`txid`) are 32-bit integers modulo $2^{32} \approx 4.29$ billion:
- Any transaction ID strictly less than $2^{31}$ transactions in the past is deemed in the past.
- If a database executes $> 2^{31}$ transactions without freezing, ancient transaction IDs appear in the "future", making historic data invisible.
- `VACUUM FREEZE` replaces ancient `xmin` values with special frozen transaction ID `FrozenTransactionId` (2), preventing catastrophic data invisibility.
