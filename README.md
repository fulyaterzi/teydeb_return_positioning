```mermaid
flowchart TD
    A([Start]) --> B[Load inputs: product list, cost params (c, F_k, G_k, H_k, s), feasible ranges (p,r,τ)]
    B --> C[Load/fit models per region r ∈ R: \n• Purchase prob D_r(k,p,r,τ)\n• Return prob R_r(k,p,r,τ)\n• (Optional) hassle h_{r,k} or features]
    C --> D{Per‑region policy allowed?}
    
    D -- Yes --> E[For each region r:\nInitialize best_r = (-∞)\nEnumerate policy k ∈ {Store, Online}]
    E --> F[Inner optimize (p, r_refund, τ)\nGrid/BO search to maximize\nΠ_r(k,p,r,τ) = M_r·D_r·(p-c-F_k − R_r·(r+G_k+H_k−s))\nsubject to constraints]
    F --> G{Capacity / CSAT constraints OK?}
    G -- No --> F
    G -- Yes --> H[Update region best: best_r ← max(best_r, Π_r, args)]
    H --> I[Store result per region:\n(k_r*, p_r*, r_r*, τ_r*, Π_r*)]
    I --> J[Aggregate KPIs across regions:\nΠ_total = Σ_r Π_r*; returns load; SLA checks]
    J --> K([Output: Region‑specific policy set\n{r: (k*, p*, r*, τ*)} + KPIs])
    
    D -- No (single global policy) --> L[Initialize best_global = (-∞)\nEnumerate policy k ∈ {Store, Online}]
    L --> M[Inner optimize (p, r_refund, τ)\nMaximize Π_total(k,p,r,τ) = Σ_r M_r·D_r·(p-c-F_k − R_r·(r+G_k+H_k−s))]
    M --> N{Capacity / CSAT constraints OK?}
    N -- No --> M
    N -- Yes --> O[Update global best: best_global ← max(best_global, Π_total, args)]
    O --> P([Output: Single policy\n(k*, p*, r*, τ*) + KPIs by region and total])
    
    style A fill:#fff,stroke:#000,stroke-width:1px
    style K fill:#eaffea,stroke:#0a0,stroke-width:1px
    style P fill:#eaffea,stroke:#0a0,stroke-width:1px
    classDef subtle fill:#f7f7f7,stroke:#333,stroke-width:1px
    class B,C,E,F,H,I,J,L,M,O subtle
