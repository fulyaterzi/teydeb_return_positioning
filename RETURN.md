```mermaid

flowchart TD
  %% ========= 1) Inputs =========
  subgraph S1[Inputs & Configuration]
    A1[Historical sales & returns]
    A2[Channels: Online / Store]
    A3[Regions & Stores]
    A4[Cost Params: c (unit cost), s (salvage), k_proc (return processing), h_store (hassle), fees]
    A5[Policy Set: BORO, BORS, BSRS, BSRO, Partial refund r<p]
    A6[Price grid P and Order Qty grid Q]
  end

  %% ========= 2) Estimation =========
  subgraph S2[Estimation & Feature Engineering]
    B1[Estimate market-demand dist F(x)]
    B2[Estimate valuation dist G(v)]
    B3[Estimate return prob Pr(return | policy, channel, region)]
    B4[Service-level & lead-time inputs]
  end

  %% ========= 3) Customer Utility =========
  subgraph S3[Customer Utility & Demand Shaping]
    C1[For each policy & channel, define customer utility U]
    C2[Online:\nU_keep = V - p\nU_return = E[V - p] + r - fee\n(choose max)]
    C3[Store:\nU_return_store = V - p - h_store + r_store]
    C4[Adoption Share α(policy,channel,region) from G(v)]
  end

  %% ========= 4) Demand under Policy =========
  subgraph S4[Policy-Conditional Demand]
    D1[Effective demand multiplier:\nθ = α(policy,channel,region)]
    D2[Demand under policy:\nX_policy ~ θ · F(x)]
    D3[Expected sales:\nE[min(X_policy,q)]]
    D4[Return rate ρ(policy,channel,region)]
  end

  %% ========= 5) Profit Model =========
  subgraph S5[Profit & Feasibility]
    E1[Revenue kept:\n(p)·(1-ρ)·E[min(X_policy,q)]]
    E2[Refunded/Salvage:\n(s - k_proc)·ρ·E[min(X_policy,q)]]
    E3[Unsold salvage:\n s·(q - E[min(X_policy,q)])]
    E4[Cost:\n -c·q]
    E5[Logistics routing:\nroute = argmin cost(hub/store/WH)\n(capacity, SLAs)]
    E6[Profit:\nΠ(policy,p,q,region,channel,route)]
  end

  %% ========= 6) Optimization =========
  subgraph S6[Optimization Loop]
    F1[Enumerate policy ∈ {BORO,BORS,BSRS,BSRO,Partial}]
    F2[Grid/solver over p ∈ P, q ∈ Q]
    F3[Compute Π for all regions, stores, channels]
    F4[Constraints:\ncap, budget, max_refund, SLA]
    F5[Select (policy*, p*, q*, routing*)\nthat maximizes total Π subject to constraints]
  end

  %% ========= 7) Deployment & Feedback =========
  subgraph S7[Deployment & Monitoring]
    G1[Publish policy & price by region/store/channel]
    G2[Operational rules:\nreturn windows, labels, routing, fees]
    G3[Track KPIs:\nconversion, return rate, NPS, cost/return, margin]
    G4[Periodic re-estimation of F, G, ρ, θ]
    G5[Experimentation:\nA/B or MAB on policies/fees/windows]
  end

  %% ========= Edges =========
  S1 --> S2
  S2 --> S3
  S3 --> S4
  S4 --> S5
  S5 --> S6
  S6 -->|policy*, p*, q*, routing*| S7
  S7 -->|new data| S2

  %% ========= Notes =========
  classDef dim fill:#f6f7fb,stroke:#c7cce1,stroke-width:1px;
  classDef bold fill:#eef7ff,stroke:#8ab6ff,stroke-width:1px;
  class S1,S2,S3,S4,S5,S6,S7 dim;
  class F5 bold;
