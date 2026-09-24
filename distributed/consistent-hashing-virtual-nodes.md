# Distributed Storage: Consistent Hashing and Virtual Nodes

## The Goal
Map keys to $N$ storage nodes such that adding or removing a node only requires moving $K / N$ keys, rather than $O(K)$ keys with traditional modulo hashing.

## The Problem of Non-Uniformity
Pure random hash points on a 32-bit ring lead to severe load imbalance (standard deviation $\approx O(1/\sqrt{N})$).

## Solution: Virtual Nodes (Vnodes)
Each physical node is assigned $V$ virtual positions (e.g. 128 to 512 vnodes) on the hash ring:
`hash("node1-vnode0")`, `hash("node1-vnode1")`, etc.
Benefits:
- Restores uniform distribution across physical nodes.
- Allows heterogeneous server capacity (more powerful machines get proportionally more vnodes).
