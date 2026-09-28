# SQLite WAL Mode: Concurrent Readers and Single Writer

## Traditional Rollback Journal
In rollback journal mode, writing locks the entire database file exclusively; readers and writers block each other.

## Write-Ahead Logging (WAL) Mode
- Original database file (`db.sqlite`) holds baseline data.
- New transactions append directly to `-wal` file.
- **WAL Index (`-shm`)**: Memory-mapped hash table in shared memory mapping database page numbers to their newest offset in the `-wal` file.
- **Concurrency**: Readers query `-shm` to read newest committed pages while a single writer appends to `-wal` simultaneously without blocking.
- **Checkpointing**: Background daemon merges pages from `-wal` back to main database file when threshold is reached.
