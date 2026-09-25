# Compilers: Tail Call Optimization (TCO)

## Definition
A call is a 'tail call' if the caller returns the result of the callee without performing any subsequent computation:
```c
int factorial_tail(int n, int acc) {
    if (n <= 1) return acc;
    return factorial_tail(n - 1, n * acc); // Pure tail call
}
```

## Machine Code Transformation
Instead of emitting `CALL` (pushing return address and allocating a new stack frame), the compiler emits `JMP`:
1. Overwrite current parameter registers with callee arguments.
2. Re-use current stack frame in-place.
3. Execution jumps directly to function entry.
Result: Reduces recursive stack usage from $O(N)$ to $O(1)$, completely preventing stack overflow errors.
