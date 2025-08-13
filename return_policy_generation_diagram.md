```mermaid

flowchart TD
    A["Start"] --> B["Load inputs: product list; cost params c, F_k, G_k, H_k, s; feasible ranges p, r, tau"]
    B --> C["Load/fit models per region r in R:<br/>• Purchase prob D_r(k,p,r,tau)<br/>• Return prob R_r(k,p,r,tau)<br/>• Optional hassle h_{r,k} or features"]
    C --> D{"Per-region policy allowed?"}
    
    D -- "Yes" --> E["For each region r:<br/>Initialize best_r = (-inf)<br/>Enumerate policy k in {Store, Online}"]
    E --> F["Inner optimize (p, r_refund, tau):<br/>Grid/BO search to maximize<br/>Pi_r(k,p,r,tau) = M_r * D_r * (p - c - F_k - R_r * (r + G_k + H_k - s))<br/>subject to constraints"]
    F --> G{"Capacity / CSAT constraints OK?"}
    G -- "No" --> F
    G -- "Yes" --> H["Update region best: best_r <- max(best_r, Pi_r, args)"]
    H --> I["Store result per region:<br/>(k_r*, p_r*, r_r*, tau_r*, Pi_r*)"]
    I --> J["Aggregate KPIs across regions:<br/>Pi_total = sum_r Pi_r*; returns load; SLA checks"]
    J --> K["Output: Region-specific policy set<br/>{r: (k*, p*, r*, tau*)} + KPIs"]
    
    D -- "No (single global policy)" --> L["Initialize best_global = (-inf)<br/>Enumerate policy k in {Store, Online}"]
    L --> M["Inner optimize (p, r_refund, tau):<br/>Maximize Pi_total(k,p,r,tau) = sum_r M_r * D_r * (p - c - F_k - R_r * (r + G_k + H_k - s))"]
    M --> N{"Capacity / CSAT constraints OK?"}
    N -- "No" --> M
    N -- "Yes" --> O["Update global best: best_global <- max(best_global, Pi_total, args)"]
    O --> P["Output: Single policy<br/>(k*, p*, r*, tau*) + KPIs by region and total"]
    
    style A fill:#ffffff,stroke:#000000,stroke-width:1px
    style K fill:#eaffea,stroke:#0a0,stroke-width:1px
    style P fill:#eaffea,stroke:#0a0,stroke-width:1px
    classDef subtle fill:#f7f7f7,stroke:#333,stroke-width:1px
    class B,C,E,F,H,I,J,L,M,O subtle
