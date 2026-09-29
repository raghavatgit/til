# PostgreSQL Indexing: GiST vs GIN

## GIN (Generalized Inverted Index)
- Inverted index mapping element values to lists of row pointers (posting lists).
- Ideal for: Full-text search (tsvector), JSONB path lookups (`?|`), and array containment (`@>`).
- Trade-off: Fast reads; slower writes due to posting list updates.

## GiST (Generalized Search Tree)
- Height-balanced search tree of arbitrary bounding predicates (e.g., bounding boxes for PostGIS spatial data, range types).
- Supports lossy index representations.
- Ideal for: Geometric queries (`ST_DWithin`), timestamp ranges (`&&`), and nearest-neighbor search (`<->`).
