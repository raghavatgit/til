# PostgreSQL VACUUM and Transaction ID Wraparound

## Dead Tuple Accumulation
Because `UPDATE` creates a new tuple and `DELETE` sets `xmax`, obsolete tuples remain in table heap pages (bloat) until cleaned up.

## VACUUM Operations
- **Standard VACUUM**: Scans pages, marks dead tuple space as available in the Free Space Map (FSM) without holding exclusive table locks.
- **VACUUM FULL**: Rewrites entire table to a new data file, reclaiming physical disk space at the expense of an exclusive table lock.

## Transaction ID Wraparound Danger
Postgres XIDs are 32-bit unsigned integers ($2^{32} \approx 4.29$ billion). If transactions exceed $2^{31}$ without vacuuming, historical transactions wrap around and appear to be in the future, rendering data invisible.
`VACUUM FREEZE` converts old XIDs to `FrozenXID` (2), which is treated as older than all possible XIDs.
