# Consistent Hashing with Bounded Loads

## The Hotspot Flaw of Standard Hashing
Even with virtual nodes, popularity skew (e.g., viral videos or hot partition keys) causes single nodes to exceed capacity while neighbor nodes sit idle.

## The Bounded Load Invariant
Defines a capacity multiplier $c > 1$ (typically $c = 1.25$ or $1.30$).
$$\text{Max Load per Node} = \lceil c \times \frac{\text{Total Requests}}{\text{Node Count}} \rceil$$

## Lookup Walk
When hashing key $k$:
1. Walk clockwise along ring to primary node $N$.
2. If $N$'s current load $< \text{Max Load}$, assign request to $N$.
3. If $N$ is overloaded, continue clockwise to next node until a node with capacity is found.
Guarantees no server is loaded beyond $(1 + \epsilon)$ of average load.
