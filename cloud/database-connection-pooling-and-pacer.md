# Database Connection Pool Sizing and Little's Law

*Date: 2026-09-17*  
*Category: Cloud & Backend / Database Architecture*

## Overview

A pervasive misconception in backend engineering is that allocating more database connections increases throughput. In reality, oversized connection pools degrade database performance due to CPU context switching, disk I/O contention, and lock serialization.

## PostgreSQL / MySQL Pool Formula

PostgreSQL and HikariCP maintainers recommend the empirical formula:

$$\text{Connections} = (\text{Core Count} \times 2) + \text{Spindle Count}$$

For an 8-core database server with fast NVMe storage:
$$\text{Optimal Connections} \approx (8 \times 2) + 1 = 17 \text{ connections}$$

## Little's Law Applied to Concurrency

Little's Law states:
$$L = \lambda \times W$$
Where $L$ is concurrent requests, $\lambda$ is arrival rate, and $W$ is average query latency.

* When 1,000 backend workers simultaneously open 1,000 database connections, the database operating system spends more CPU cycles switching execution contexts between processes (`kswapd`, `postgres` backends) than actually executing query plans.
* Queuing requests inside the application layer (e.g. HikariCP or pgbouncer) keeps the database CPU execution queues shallow, dramatically shortening average query latency $W$ and maximizing total throughput.
