# LWW-Element-Set: State-Based CRDT

## Mathematical Structure
Composed of an Add Set $A$ and a Remove Set $R$, where each element is tagged with a monotonic timestamp:
$$A = \{ (e, t_a) \}, \quad R = \{ (e, t_r) \}$$

## Membership Invariant
An element $e$ is a member of the set if and only if:
$$e \in A \land (e \notin R \lor t_a > t_r)$$
Ties ($t_a = t_r$) are broken deterministically (e.g., add bias).

## Merge Operator
$$A_{\text{merged}} = A_1 \cup A_2, \quad R_{\text{merged}} = R_1 \cup R_2$$
Guarantees strong eventual consistency across all replicas regardless of message reordering.
