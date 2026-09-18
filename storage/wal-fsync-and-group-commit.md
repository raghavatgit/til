# Storage: Write-Ahead Log Durability, fsync, and Group Commit

## The Page Cache vs Mechanical/Flash Media
When an application calls `write(2)` to an append-only WAL file:
- Data is written to the OS Kernel Page Cache, not persistent NAND flash.
- If power fails, unwritten dirty pages are lost, corrupting ACID transaction durability.
- Calling `fsync(2)` or `fdatasync(2)` forces the disk controller to flush volatile DRAM write caches to non-volatile media.

## The Cost of fsync
A single `fsync(2)` operation takes between 0.5ms and 5ms on modern NVMe / enterprise SSDs due to hardware queue barriers and flash erase block write cycles:
- Naive design: 1 `fsync` per client transaction limits throughput to 200 - 2000 transactions/sec per core.

## Group Commit Architecture
Group commit decouples client transaction completion from individual disk flushes:
1. Incoming transactions append their log records to an in-memory WAL buffer.
2. The first thread in a batch acts as the **batch leader** and initiates `fdatasync(2)`.
3. Concurrently arriving transactions register as **batch followers** and wait on a condition variable.
4. When the single `fdatasync(2)` call returns, all grouped transactions are marked committed simultaneously.
5. Increases transaction throughput by $10\times$ to $50\times$ while maintaining strict durability invariants.
