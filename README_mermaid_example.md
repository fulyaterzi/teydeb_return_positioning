# Region‑Aware Return Policy Optimization — Flow

The diagram below is embedded as Mermaid and **renders directly on GitHub** (in Markdown).  
If you still see plain code, see the troubleshooting notes at the bottom.

```mermaid
flowchart TD
    A([Start]) --> B[Load inputs: product list, cost params (c, F_k, G_k, H_k, s), feasible ranges (p,r,τ)]
    B --> C[Load/fit models per region r ∈ R: 
• Purchase prob D_r(k,p,r,τ)
• Return prob R_r(k,p,r,τ)
• (Optional) hassle h_{r,k} or features]
    C --> D{Per‑region policy allowed?}
    
    D -- Yes --> E[For each region r:
Initialize best_r = (-∞)
Enumerate policy k ∈ {Store, Online}]
    E --> F[Inner optimize (p, r_refund, τ)
Grid/BO search to maximize
Π_r(k,p,r,τ) = M_r·D_r·(p-c-F_k − R_r·(r+G_k+H_k−s))
subject to constraints]
    F --> G{Capacity / CSAT constraints OK?}
    G -- No --> F
    G -- Yes --> H[Update region best: best_r ← max(best_r, Π_r, args)]
    H --> I[Store result per region:
(k_r*, p_r*, r_r*, τ_r*, Π_r*)]
    I --> J[Aggregate KPIs across regions:
Π_total = Σ_r Π_r*; returns load; SLA checks]
    J --> K([Output: Region‑specific policy set
{r: (k*, p*, r*, τ*)} + KPIs])
    
    D -- No (single global policy) --> L[Initialize best_global = (-∞)
Enumerate policy k ∈ {Store, Online}]
    L --> M[Inner optimize (p, r_refund, τ)
Maximize Π_total(k,p,r,τ) = Σ_r M_r·D_r·(p-c-F_k − R_r·(r+G_k+H_k−s))]
    M --> N{Capacity / CSAT constraints OK?}
    N -- No --> M
    N -- Yes --> O[Update global best: best_global ← max(best_global, Π_total, args)]
    O --> P([Output: Single policy
(k*, p*, r*, τ*) + KPIs by region and total])
    
    style A fill:#fff,stroke:#000,stroke-width:1px
    style K fill:#eaffea,stroke:#0a0,stroke-width:1px
    style P fill:#eaffea,stroke:#0a0,stroke-width:1px
    classDef subtle fill:#f7f7f7,stroke:#333,stroke-width:1px
    class B,C,E,F,H,I,J,L,M,O subtle
```

## How to use on GitHub
- Put this content in a file like `README.md` or any `.md` file in your repo.
- Make sure the **fence starts with** three backticks and `mermaid` (no leading spaces).
- View the file in the repo (not Raw). GitHub will render the diagram.

## Troubleshooting
1. **Enterprise Server**: Your GitHub Enterprise may have Mermaid disabled. Ask an admin to enable Mermaid diagrams in the site settings.
2. **Old Markdown renderer**: In some self‑hosted setups, Mermaid isn’t supported yet.
3. **Indentation**: Don’t indent the opening ```mermaid fence; leading spaces break rendering.
4. **Browser extensions**: Some privacy/script blockers can block rendering; try a clean browser session.
