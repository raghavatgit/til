# ANSI SQL Isolation Levels and Transaction Anomalies

## Anomalies
1. **Dirty Read**: Transaction reads uncommitted changes from another transaction.
2. **Non-Repeatable Read**: Re-reading a row returns altered values due to concurrent commit.
3. **Phantom Read**: Re-executing a range query returns newly inserted rows satisfying the predicate.
4. **Write Skew**: Two concurrent transactions read intersecting state and update disjoint records violating a global constraint.

## Isolation Levels Matrix
| Isolation Level | Dirty Read | Non-Repeatable Read | Phantom Read | Write Skew |
| :--- | :--- | :--- | :--- | :--- |
| Read Uncommitted | Permitted | Permitted | Permitted | Permitted |
| Read Committed | Prevented | Permitted | Permitted | Permitted |
| Repeatable Read | Prevented | Prevented | Prevented (Snapshot) | Permitted |
| Serializable | Prevented | Prevented | Prevented | Prevented |
