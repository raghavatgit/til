# AVX-512 SIMD Vectorization and Frequency Scaling

## 512-Bit Vector Registers (ZMM)
AVX-512 processes sixteen 32-bit floats simultaneously in a single CPU instruction cycle.

## Downclocking Side-Effects
On older Intel microarchitectures (Skylake-X), activating 512-bit vector units causes transient voltage drops. The CPU automatically lowers its clock frequency by 10-20% across all cores executing the wide instructions. Modern CPUs (Zen 4, Golden Cove) mitigate this using dual-pumped 256-bit pipelines.
