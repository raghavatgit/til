# Database Query Engines: Index Skip Scan

## Traditional Composite Index Limitation
Given an index on `(gender, registration_date)`:
Standard B-Tree lookups require specifying the leading index column (`gender`) to perform a range scan on `registration_date`. A query filtering only `WHERE registration_date > '2026-01-01'` traditionally causes a full table scan.

## Skip Scan Mechanics
When the leading column has very low cardinality (e.g. `gender` having only `'M'`, `'F'`, `'O'`):
The query engine rewrites the single query into distinct parallel index sub-scans:
1. Scan `('M', registration_date > '2026-01-01')`
2. Scan `('F', registration_date > '2026-01-01')`
3. Scan `('O', registration_date > '2026-01-01')`
Merges results without reading non-matching rows, preserving index utility.
