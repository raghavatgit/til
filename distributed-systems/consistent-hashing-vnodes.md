# Consistent Hashing with Virtual Nodes

## The Data Skew Problem
With standard consistent hashing, random hashing of physical nodes onto the 32-bit ring results in non-uniform partition sizes (some nodes handle 3x more traffic than others).

## Virtual Nodes (vnodes)
Each physical node is assigned K virtual nodes (typically K = 128 - 256) scattered across the ring. When a physical node joins or leaves, traffic is redistributed uniformly in small slices across all surviving nodes.
