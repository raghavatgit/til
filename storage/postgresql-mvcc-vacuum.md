# PostgreSQL MVCC and Table Bloat

## Implementation
PostgreSQL does not overwrite rows in-place. An `UPDATE` writes a new row version with `xmin = current_tx` and marks the old row with `xmax = current_tx`.

## VACUUM Mechanics
Old row versions (dead tuples) remain on disk until `VACUUM` identifies that no active transaction possesses an `xmin` earlier than the dead tuple's `xmax`. `VACUUM` marks the space in the Free Space Map (FSM) for future row inserts.
