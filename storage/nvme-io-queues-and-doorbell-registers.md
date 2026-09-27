# NVMe Submission / Completion Queues & Doorbell Registers

## Replacing Legacy SATA AHCI
SATA AHCI was limited to a single command queue of 32 depth. NVMe (Non-Volatile Memory Express) supports up to 64,000 queues, each holding up to 64,000 commands.

---

## Lockless Multi-Queue Architecture
* **Per-CPU Queues:** Each CPU core owns a dedicated Submission Queue (SQ) and Completion Queue (CQ) in host DRAM, eliminating cross-core lock contention.
* **Doorbell Registers:** To submit commands, the CPU writes to a memory-mapped PCIe Doorbell register on the NVMe controller, alerting hardware without polling.
