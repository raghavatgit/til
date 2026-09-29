# Conflict-Free Replicated Data Types: PN-Counter

## State-Based CRDT Formulation
A Positive-Negative Counter (PN-Counter) maintains two monotonically increasing vector clocks per node:
- P: Vector of increments
- N: Vector of decrements

```rust
pub struct PNCounter {
    p: Vec<u64>,
    n: Vec<u64>,
}

impl PNCounter {
    pub fn value(&self) -> i64 {
        let positive: u64 = self.p.iter().sum();
        let negative: u64 = self.n.iter().sum();
        (positive as i64) - (negative as i64)
    }

    pub fn merge(&mut self, other: &PNCounter) {
        for i in 0..self.p.len() {
            self.p[i] = self.p[i].max(other.p[i]);
            self.n[i] = self.n[i].max(other.n[i]);
        }
    }
}
```
