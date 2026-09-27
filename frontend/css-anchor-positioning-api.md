# CSS Anchor Positioning API

## Eliminating JavaScript Geometry Calculations
Positioning floating elements (tooltips, dropdowns, context menus) relative to an anchor historically required JavaScript `getBoundingClientRect()` loops and scroll listeners.

---

## Native Declarative CSS Anchoring
```css
/* Anchor element */
.anchor-btn {
  anchor-name: --menu-trigger;
}

/* Floating popover element */
.dropdown-menu {
  position: fixed;
  position-anchor: --menu-trigger;
  top: anchor(bottom);
  left: anchor(left);
  position-try-fallbacks: flip-block, flip-inline;
}
```

The browser handles viewport boundary clipping, collision detection, and scrolling coordinates entirely on the compositor thread.
