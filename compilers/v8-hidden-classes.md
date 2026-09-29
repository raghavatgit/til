# V8 Hidden Classes (Maps) and Inline Caching

## Mechanism
JavaScript is dynamically typed. V8 assigns an internal hidden class (`Map`) to each object:
1. `const pt = {};` -> Map M0.
2. `pt.x = 1;` -> Transition M0 -> M1 (adds offset for x).
3. `pt.y = 2;` -> Transition M1 -> M2 (adds offset for y).

## Polymorphic vs Monomorphic Inline Caching (IC)
- Monomorphic: Call site encounters objects with identical Maps. Direct property lookup via cached field offset (sub-nanosecond).
- Megamorphic: Call site encounters > 4 distinct Maps. Falls back to slow dictionary hash lookup.
