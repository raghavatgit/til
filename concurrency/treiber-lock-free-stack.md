# Treiber Lock-Free Stack Architecture

## Concept
A Treiber stack uses atomic compare-and-swap (CAS) primitives on the head pointer to achieve thread-safe, lock-free LIFO operations.

## The ABA Problem
If thread T1 reads head A, gets descheduled, while thread T2 pops A, pops B, and pushes A back, T1's CAS will succeed even though the stack state has changed fundamentally.

## Mitigation Strategies
1. Tagged Pointers: Coupling an atomic 16-bit sequence counter with the 48-bit virtual address pointer (`AtomicUsize` or double-word CAS `CMPXCHG16B`).
2. Hazard Pointers: Reader threads register active memory pointers to prevent premature memory reclamation.
3. Epoch-Based Reclamation (EBR): Memory is retired into epoch lists and freed only when all threads advance past the retired epoch.
