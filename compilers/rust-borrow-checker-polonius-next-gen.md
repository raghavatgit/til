# Rust Polonius: Next-Generation Borrow Checker

## Limitations of Non-Lexical Lifetimes (NLL)
NLL uses Control Flow Graph (CFG) analysis, but struggles with certain patterns where references flow through data structures or where loans should end based on values rather than lexical locations.

---

## Polonius Datalog Representation
Polonius reformulates the borrow checker as a set of declarative logic rules in Datalog. It tracks "origins" (subsets of loans that may be referenced) at every program point, accepting safe programs previously rejected by NLL without sacrificing safety invariants.
