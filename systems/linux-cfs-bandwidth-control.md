# Linux CFS Bandwidth Controller

## Algorithmic Design
CPU bandwidth controller divides execution time into epochs. Threads acquire quota in 5ms slices from the global pool.
Burstable Quotas (`cpu.cfs_burst_us`): Allows containers to accumulate unused quota across idle periods to absorb sudden traffic spikes without throttling.
