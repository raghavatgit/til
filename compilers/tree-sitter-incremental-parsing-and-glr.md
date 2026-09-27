# Tree-sitter Incremental Parsing & GLR Grammar

## Real-Time Syntax Tree Updates
Traditional parsers re-parse the entire source file from scratch on every keystroke. Tree-sitter parses incrementally in sub-millisecond intervals.

---

## GLR & Tree Diffing
* **Generalized LR (GLR):** Handles grammar ambiguities by splitting parser state branches and reconciling them when tokens resolve.
* **Syntax Tree Re-use:** When code edits occur, Tree-sitter edits node byte offsets and re-parses only the modified syntax branches, preserving identical AST subtrees without memory reallocation.
