# Database Query Optimization: Cost-Based Optimizers (CBO)

## Optimization Pipeline
1. Parser generates Abstract Syntax Tree (AST).
2. Rewriter applies rule-based relational algebra transformations (predicate pushdown, projection pruning).
3. Optimizer evaluates execution plan alternatives and estimates total cost.

## Cost Estimation Formula
$$\text{Cost} = (N_{\text{page\_io}} \times C_{\text{page}}) + (N_{\text{cpu\_tuples}} \times C_{\text{tuple}}) + (N_{\text{cpu\_operators}} \times C_{\text{op}})$$

## Join Enumeration
- Uses Dynamic Programming (System-R algorithm) for small joins ($< 10$ tables).
- Falls back to Genetic Query Optimization (GEQO) when join combinations exceed computational thresholds.
