# Loop Invariant Code Motion (LICM)

## Optimization
Calculations inside a loop whose operands never change across iterations are hoisted into the loop pre-header block, executing once instead of N times.

## Safety Criteria
An instruction `s: x = a + b` can be hoisted out of loop L only if:
1. `s` dominates all loop exit blocks.
2. No other statement in L defines `x`.
3. All uses of `x` in L are dominated by `s`.
