# Kubernetes CFS Quota Throttling

## The 100ms Period Window
Kubernetes sets CFS CPU limits using `cpu.cfs_quota_us` and `cpu.cfs_period_us` (default 100,000us = 100ms). A container with limit 1.0 CPU gets 100ms of CPU time per 100ms period.

## Throttling Burst
If a multi-threaded process spawns 8 threads that run for 12.5ms, all 100ms of quota are consumed in 12.5ms. The container is throttled for the remaining 87.5ms of the period, causing high latency spikes.
