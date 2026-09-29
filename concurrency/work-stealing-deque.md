# Chase-Lev Work-Stealing Deque

## Architecture
The Chase-Lev deque is optimized for fork-join task parallelism:
- Owner Thread: Pushes and pops tasks from the bottom of the deque using relaxed atomic operations (LIFO order, maximizing CPU cache locality).
- Thief Threads: Steal tasks from the top of the deque using CAS atomic synchronization (FIFO order, stealing large chunks of work).

## Memory Ordering Invariants
- `push()`: Owner updates `bottom` with `Ordering::Release`.
- `pop()`: Owner decrements `bottom` with `Ordering::SeqCst` and uses CAS if `bottom <= top`.
- `steal()`: Thieves load `top` with `Ordering::Acquire` and compete with CAS on `top`.
