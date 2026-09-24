# Database Concurrency: MVCC Visibility Rules

## Core Value Proposition
Readers never block writers, and writers never block readers.

## Implementation Mechanics
- **PostgreSQL**: Stores old tuple versions directly in table heap with `xmin` (creating transaction ID) and `xmax` (deleting transaction ID) headers. Vacuum cleaner purges dead tuples.
- **MySQL InnoDB**: Overwrites row in clustered index in-place and stores delta reversal logs in Undo Segments. Readers construct past snapshots by traversing the undo chain backwards.

## Snapshot Isolation
A query reads tuples where `xmin` was committed before query start, and `xmax` is either uncommitted or committed after query start.
