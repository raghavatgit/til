# Hardware-Accelerated 3D Perspective Transforms in CSS

*Date: 2026-09-07*  
*Category: Frontend Engineering / Spatial UI*

## Overview

Creating dynamic card tilt effects (as demonstrated in ContactMe and Zenflow-3D) does not require heavy runtime libraries like Three.js. Standard browser CSS transforms provide GPU-accelerated matrix manipulation with sub-50 KB total asset footprints.

## Mathematical Model

Given cursor coordinates `(clientX, clientY)` and the target element bounding rect `(left, top, width, height)`:

1. Normalize cursor coordinates from `-1.0` to `1.0` relative to the element center:
   ```javascript
   const rect = element.getBoundingClientRect();
   const centerX = rect.left + rect.width / 2;
   const centerY = rect.top + rect.height / 2;
   
   const normX = (e.clientX - centerX) / (rect.width / 2);
   const normY = (e.clientY - centerY) / (rect.height / 2);
   ```

2. Calculate rotation angles:
   ```javascript
   const maxTilt = 15; // degrees
   const rotateX = -normY * maxTilt;
   const rotateY = normX * maxTilt;
   ```

3. Apply transform with `perspective` to avoid layout thrashing:
   ```css
   .card {
     transform: perspective(1000px) rotateX(var(--rot-x)) rotateY(var(--rot-y));
     will-change: transform;
   }
   ```
