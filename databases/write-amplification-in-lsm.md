# LSM-Tree Performance: Write Amplification Factor (WAF)

## Definition
$$\text{WAF} = \frac{\text{Bytes written to persistent storage}}{\text{Bytes written by application}}$$

## Causes of Write Amplification
1. Initial append to Write-Ahead Log (WAL) = $1\times$.
2. Memtable flush to Level 0 SSTables = $1\times$.
3. Compaction merges Level $L$ into Level $L+1$: In Leveled compaction, every byte is rewritten approximately $10\times$ per level.
Overall WAF in Leveled compaction frequently exceeds $20-40\times$, inducing high SSD write wear.

## Trade-off with Size-Tiered
Size-Tiered compaction yields much lower WAF ($pprox 4-8\times$), but suffers from high Space Amplification and severe temporary latency spikes during large merges.
