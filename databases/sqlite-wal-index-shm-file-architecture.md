# SQLite WAL-Index (`.shm`) Architecture

## Bypassing Disk Reads in WAL Mode
In SQLite WAL mode, updates append to the `database.db-wal` file. If readers had to scan the raw WAL file on disk to determine whether an updated page exists, read performance would plummet.

---

## The Shared Memory (`.shm`) File
SQLite creates a `.shm` file mapped into the virtual memory of all concurrent reader processes:
* Acts as an in-memory hash table indexing page numbers present in the WAL file.
* Readers execute lock-free binary searches in shared memory to instantly find the latest WAL page offset without touching disk.
* Atomic coordination achieved via POSIX shared memory locks (`fcntl`).
