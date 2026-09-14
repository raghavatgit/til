# Sentinel-Based Viewport Virtualization via IntersectionObserver

*Date: 2026-09-14*  
*Category: Frontend Engineering / Web Performance*

## Overview

Traditional infinite scrolling and viewport virtualization relied on binding `window.addEventListener('scroll', handler)`. Even when throttled via `requestAnimationFrame`, scroll event handlers run synchronously on the browser's main thread and risk layout thrashing when reading `element.getBoundingClientRect()`.

Modern web performance leverages the `IntersectionObserver` API to asynchronously track when boundary sentinel elements enter or exit the active viewport.

## Sentinel Implementation Pattern

```typescript
export function setupInfiniteFeed(
  sentinelElement: HTMLElement,
  loadNextPage: () => Promise<void>,
  rootMargin: string = '200px'
): () => void {
  let isFetching = false;

  const observer = new IntersectionObserver(
    async (entries) => {
      const target = entries[0];
      if (target.isIntersecting && !isFetching) {
        isFetching = true;
        try {
          await loadNextPage();
        } finally {
          isFetching = false;
        }
      }
    },
    {
      root: null, // Relative to browser viewport
      rootMargin, // Pre-fetch 200px before reaching the physical bottom
      threshold: 0.1
    }
  );

  observer.observe(sentinelElement);

  // Return teardown function
  return () => observer.disconnect();
}
```

## Performance Benefits
* **Offloaded Layout Calculations:** Intersection tests are computed asynchronously by the browser compositing pipeline outside the main JavaScript execution loop.
* **Predictive Pre-Fetching:** `rootMargin: '200px'` initiates network requests before the user reaches the absolute bottom boundary, providing perceived zero-latency pagination.
* **Memory Conservation:** Unrendered offscreen items can swap into placeholder height anchors to maintain smooth native scrollbar proportions without keeping thousands of heavy DOM nodes active.
