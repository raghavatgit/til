# Browser Back/Forward Cache (bfcache) Preservation and Paint Holding

*Date: 2026-09-17*  
*Category: Frontend Engineering / Web Performance*

## Overview

The Back/Forward Cache (bfcache) is an in-memory snapshot mechanism implemented in modern browsers (Chrome, Safari, Firefox) that pauses and preserves complete JavaScript heap, DOM tree, and render state when users navigate away. Returning to a page results in instant restoration (0ms load time).

## Practices that Evict Pages from bfcache

1. **`unload` Event Listeners:** Attaching `window.addEventListener('unload', ...)` unconditionally disqualifies the page from bfcache in Chrome and Firefox. Use `pagehide` instead:
   ```javascript
   window.addEventListener('pagehide', (event) => {
     if (event.persisted) {
       // Page entered bfcache
     }
   });
   ```
2. **Unclosed IndexedDB or Web Lock Connections:** Open transaction locks prevent memory freeze.
3. **`Cache-Control: no-store`:** Instructs browsers never to cache pages in memory.

## Paint Holding
Modern Chromium engines implement **Paint Holding** during same-origin navigations, deferring clearing the previous screen buffer until the incoming page paints its first frame. This completely eliminates white flash artifacts during navigation.
