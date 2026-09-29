# AVX-512 SIMD Vectorization and Frequency Scaling

Utilizing 512-bit wide vector registers requires awareness of CPU power state transitions and license levels.

## Frequency Levels
- **License 0 (SSE/Normal)**: Base and turbo frequencies maintained.
- **License 1 (AVX2)**: Slight voltage increase; minor downclock on older Intel microarchitectures.
- **License 2 (AVX-512 Heavy)**: Substantial voltage increase; core downclocks to protect thermal limits.
