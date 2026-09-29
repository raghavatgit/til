# Database Buffer Management: Clock Sweep and 2Q Eviction

## Standard LRU Flaws in Databases
Sequential table scans (`SELECT * FROM huge_table`) wipe out the entire buffer cache, evicting frequently accessed index pages (Buffer Cache Pollution).

## Clock Sweep Algorithm (Second Chance)
- Maintains circular array of buffer page headers with a usage counter.
- Pointer sweeps circular list:
  - If usage count $> 0$, decrements count and advances pointer.
  - If usage count $== 0$ and page is not pinned, evicts page.

## 2Q (Two Queue) Enhancement
Separates cache into:
1. First-in FIFO queue for single-access scan pages.
2. Main LRU queue for pages accessed multiple times, shielding hot working sets from scan pollution.
