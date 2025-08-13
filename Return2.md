```mermaid
flowchart TD
    A[Inputs sales returns costs price salvage] --> B[Estimate demand F]
    B --> C[Estimate valuation G]
    C --> D[Return probability under full refund]
    D --> E[Expected sales from demand and order quantity]
    E --> F[Profit calculation for full refund]
    F --> G[Choose order quantity q star]
    G --> H[Apply full refund policy]
    H --> I[Monitor kpi and update]

