# MySQL InnoDB Change Buffer & Adaptive Hash Index

## Change Buffer (Insert Buffer)
When secondary index pages are not present in the InnoDB Buffer Pool, modifying them would require synchronous random disk I/O. InnoDB buffers secondary index changes in the Change Buffer and merges them asynchronously when the target page is loaded into memory or during purge operations.

---

## Adaptive Hash Index (AHI)
InnoDB monitors B+ Tree index searches. If patterns indicate repetitive point lookups on specific leaf page prefixes, InnoDB automatically builds an in-memory hash index in the buffer pool, accelerating lookups from O(log N) tree descents to O(1) hash queries.
