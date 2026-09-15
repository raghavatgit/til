# CSS Subgrid for Nested Multi-Card Alignments

*Date: 2026-09-15*  
*Category: Frontend Engineering / CSS Layouts*

## Overview

In traditional CSS Grid card layouts, each card is an independent grid formatting context. When card headers or descriptions have variable content lengths, matching the vertical baseline of buttons across adjacent cards required complex JavaScript height watchers (`ResizeObserver`) or nested flexbox hacks.

CSS Subgrid (`grid-template-rows: subgrid`) allows child cards to inherit the parent grid's row definitions directly.

## Implementation Pattern

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  grid-auto-rows: auto 1fr auto; /* Title, Body, Actions */
  gap: 24px;
}

.card {
  display: grid;
  grid-row: span 3;
  grid-template-rows: subgrid;
}

.card-title {
  /* Aligns automatically across all cards in the same row */
}

.card-body {
  /* Expands to match the tallest sibling card */
}

.card-actions {
  /* Pins consistently to the bottom */
}
```

## Key Benefit
Eliminates layout shift and synchronizes row tracks across responsive columns purely in CSS with zero JavaScript overhead.
