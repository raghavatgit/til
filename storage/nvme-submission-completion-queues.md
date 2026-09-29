# NVMe Hardware Multi-Queue Architecture

## Architectural Evolution
Legacy SATA/AHCI controllers relied on a single command queue of depth 32 protected by a global controller lock. NVMe supports up to 64,000 paired Submission (SQ) and Completion Queues (CQ), each with a depth of 64,000 commands.

## Core Affinity
By assigning a dedicated SQ/CQ pair to each CPU core, NVMe eliminates lock contention across cores, enabling millions of IOPS per device.
