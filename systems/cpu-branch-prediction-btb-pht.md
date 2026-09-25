# Microarchitecture: CPU Branch Prediction and Speculative Flushes

## Hardware Predictor Components
1. **Branch Target Buffer (BTB)**: Cache mapping branch instruction addresses to target destination addresses.
2. **Pattern History Table (PHT)**: 2-bit saturating counters (Strongly Not Taken, Weakly Not Taken, Weakly Taken, Strongly Taken) tracking historical outcomes.
3. **Two-Level Adaptive Predictors**: Use a global branch history register (BHR) to correlate outcomes across multiple adjacent branches.

## Pipeline Bubble Penalty
When a branch is mispredicted:
- Modern deep pipelines (14 to 20 stages on x86-64) must flush all speculatively decoded, renamed, and executed instructions.
- Cost: 15 to 25 wasted clock cycles per branch misprediction.
- Software optimization: Sort arrays prior to processing, or use branchless arithmetic (e.g. conditional moves `CMOV`).
