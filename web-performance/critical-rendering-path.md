# Critical Rendering Path and Layout Thrashing

## DOM and CSSOM Construction
1. HTML Parsing -> DOM Tree.
2. CSS Parsing -> CSSOM Tree.
3. DOM + CSSOM -> Render Tree.
4. Layout (Reflow): Calculates exact geometric coordinates for visible elements.
5. Paint: Rasterizes pixel bitmaps into GPU layers.

## Layout Thrashing
Reading geometric properties (`offsetWidth`, `scrollTop`) immediately after mutating the DOM forces the browser to synchronously trigger reflow before the next animation frame, causing frame drops.
