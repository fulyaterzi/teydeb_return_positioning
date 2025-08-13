```mermaid
flowchart TD
    A[Input Data: sales, returns, cost, price, salvage value] --> B[Estimate demand distribution F]
    B --> C[Estimate customer valuation distribution G]
    C --> D[Compute return probability under full refund]
    D --> E[Calculate expected sales = E[min(X, q)]]
    E --> F[Profit = (p - s) * (1 - return rate) * expected sales + s * (q - expected sales) - c * q]
    F --> G[Optimize order quantity q to maximize profit]
    G --> H[Select policy: Full Refund]
    H --> I[Deploy policy and monitor results]
