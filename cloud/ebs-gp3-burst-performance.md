# Cloud Block Store Performance: GP3 vs GP2 IOPS

## GP2 Burst Bucket Architecture
GP2 IOPS scales with volume size (3 IOPS per GB). Small volumes rely on a token bucket of 5.4 million credits to burst to 3,000 IOPS. Once credits exhaust, throughput crashes to baseline.

## GP3 Decoupled Provisioning
GP3 decouples storage capacity from performance: 3,000 IOPS and 125 MB/s baseline are included free regardless of volume size, with independent scaling up to 16,000 IOPS.
