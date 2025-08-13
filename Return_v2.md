```mermaid

%%{init: {'flowchart': {'htmlLabels': true, 'wrap': true, 'nodeSpacing': 30, 'rankSpacing': 40}} }%%
flowchart LR
    A["Start"] --> B["Inputs:<br/>• Products<br/>• Costs c, F<sub>k</sub>, G<sub>k</sub>, H<sub>k</sub>, s<br/>• Ranges p, r, &tau;"]
    B --> C["Per‑region models:<br/>• Purchase D<sub>r</sub>(k,p,r,&tau;)<br/>• Return R<sub>r</sub>(k,p,r,&tau;)<br/>• (Optional) hassle h<sub>r,k</sub>"]
    C --> D{"Per‑region policy?"}

    D -- "Yes" --> E["For each region r:<br/>init best<sub>r</sub><br/>enumerate k ∈ {Store, Online}"]
    E --> F["Optimize (p, r<sub>refund</sub>, &tau;)<br/>Max &Pi;<sub>r</sub> = M<sub>r</sub>·D<sub>r</sub>·(p−c−F<sub>k</sub> − R<sub>r</sub>·(r+G<sub>k</sub>+H<sub>k</sub>−s))<br/>s.t. constraints"]
    F --> G{"Capacity / CSAT OK?"}
    G -- "No" --> F
    G -- "Yes" --> H["Update best<sub>r</sub>"]
    H --> I["Store region result:<br/>(k<sub>r</sub>* , p<sub>r</sub>* , r<sub>r</sub>* , &tau;<sub>r</sub>* , &Pi;<sub>r</sub>*)"]
    I --> J["Aggregate KPIs:<br/>&Pi;<sub>total</sub> = Σ<sub>r</sub>&Pi;<sub>r</sub>*; returns load; SLA"]
    J --> K["Output:<br/>{r → (k*, p*, r*, &tau;*)} + KPIs"]

    D -- "No (global)" --> L["Init best<sub>g</sub><br/>enumerate k ∈ {Store, Online}"]
    L --> M["Optimize (p, r<sub>refund</sub>, &tau;)<br/>Max &Pi;<sub>total</sub> = Σ<sub>r</sub> M<sub>r</sub>·D<sub>r</sub>·(p−c−F<sub>k</sub> − R<sub>r</sub>·(r+G<sub>k</sub>+H<sub>k</sub>−s))"]
    M --> N{"Capacity / CSAT OK?"}
    N -- "No" --> M
    N -- "Yes" --> O["Update best<sub>g</sub>"]
    O --> P["Output:<br/>(k*, p*, r*, &tau;*) + KPIs by region & total"]

    %% Styling
    classDef small fill:#f7f7f7,stroke:#333,stroke-width:1px,font-size:11px
    class B,C,E,F,H,I,J,L,M,O small
    style A fill:#ffffff,stroke:#000000,stroke-width:1px
    style K fill:#eaffea,stroke:#0a0,stroke-width:1px
    style P fill:#eaffea,stroke:#0a0,stroke-width:1px
