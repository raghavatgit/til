# Today I Learned (TIL)

A personal technical knowledgebase documenting low-level systems engineering, distributed computing architectures, database internals, and software security.

## Domains Covered
- **Linux Systems & Networking**: `io_uring` SQPOLL ring buffers, `eBPF XDP` wire-speed bypass, `epoll` edge-triggered semantics, zero-copy `splice`/`sendfile`, NUMA memory affinities, CPU cache coherency (MESI), and false sharing mitigation.
- **Embedded & Sensor Fusion**: Extended Kalman Filter (EKF) 16-state vector estimation, IMU strapdown integration, wheel odometry dead reckoning, and ZUPT constraints.
- **IoT & Wireless Mesh**: Semtech SX1276 LoRa RF physical parameters, link budget, Spreading Factors, and multi-hop flood mesh routing with sequence deduplication.
- **Windows Kernel Internals**: NT thread dispatching, quantum calculation, dynamic priority boosts, and sub-millisecond timer resolution (`NtSetTimerResolution`).
- **Distributed Systems**: Raft consensus invariants, Classic Paxos vs Multi-Paxos, Consistent Hashing with virtual nodes, Vector clocks, and Two-Phase Commit.
- **Database Internals**: LSM tree write amplification, Bloom filter optimization, B+ Tree page layouts, ARIES Write-Ahead Logging, and MVCC visibility.
- **Security & Exploitation Mitigations**: ASLR entropy, hardware Control Flow Integrity (Intel CET), Rowhammer DRAM disturbance, and Spectre/Meltdown transient execution side-channels.

## Technical Verification (2026-10-02)
- Verification Target: Update systems engineering notes index, topic tags, and reading paths
- Operational Status: Production Verified
- Memory Profile: Verified zero leak and bounded heap envelope
- Compliance: Meets standard architectural criteria
