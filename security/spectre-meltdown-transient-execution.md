# Hardware Security: Speculative Execution Side-Channels

## Meltdown (Rogue Data Load)
Takes advantage of out-of-order execution:
CPU speculatively executes instructions that read kernel memory before user-kernel privilege check completes. The architectural registers are rolled back on fault, but microarchitectural CPU cache lines remain modified, allowing byte recovery via flush-and-reload timing.
Mitigated via Kernel Page Table Isolation (KPTI).

## Spectre (Branch Target Injection & Bounds Check Bypass)
Trains CPU branch predictors (Branch Target Buffer or Pattern History Table) to mispredict branches speculatively, accessing secret data past boundary checks and exfiltrating via cache side-channels.
Mitigated via Retpolines and `spec_ctrl` microcode barriers.
