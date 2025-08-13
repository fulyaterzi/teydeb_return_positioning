```mermaid

flowchart TD
  %% ========= 1) Inputs =========
  subgraph S1[Inputs & Configuration]
    A1[Historical sales and returns]
    A2[Channels: Online or Store]
    A3[Regions and Stores]
    A4[Cost Parameters: unit cost c, salvage s, processing cost k_proc, hassle cost h_store, fees]
    A5[Policy Set: BORO, BORS, BSRS, BSRO, Partial refund r<p]
    A6[Price grid P and Order Qty grid Q]
  end

  %% ========= 2) Estimation =========
  subgraph S2[Estimation and Feature Engineering]
    B1[Estimate market demand distribution F(x)]
    B2[Estimate valuation distribution G(v)]
    B3[Estimate return probability by policy, channel, region]
    B4[Service level and lead time inputs]
  end

  %% ========= 3) Customer Utility =========
  subgraph S3[Customer Utility and Demand Shaping]
    C1[For each policy and channel, define customer utility U]
    C2[Online: U_keep = V - p; U_return = E[V - p] + r - fee]
    C3[Store: U_return_store = V - p - h_store + r_store]
    C4[Adoption share alpha(policy,channel,region) from G(v)]
  end

  %% ========= 4) Demand under Policy =========
  subgraph S4[Policy Conditional Demand]
    D1[Effective demand multiplier theta = adoption share]
    D2[Demand under policy: X_policy = theta * F(x)]
    D3[Expected sales: E[min(X_policy,q)]]
    D4[Return rate rho(policy,channel,region)]
  end

  %% ========= 5) Profit Model =========
  subgraph S5[Profit and Feasibility]
    E1[Revenue kept = p * (1 - rho) * E[min(X_policy,q)]]
    E2[Refunded or salvage = (s - k_proc) * rho * E[min(X_policy,q)]]
    E3[Unsold salvage = s * (q - E[min(X_policy,q)])]
    E4[Cost = -c * q]
    E5[Logistics routing: choose hub, store, or warehouse with lowest cost and meets SLA]
    E6[Profit = sum of all terms above]
  end

  %% ========= 6) Optimization =========
  subgraph S6[Optimization Loop]
    F1[Enumerate policy options]
    F2[Search over price P and order quantity Q]
    F3[Compute profit for all regions, stores, channels]
    F4[Check constraints: capacity, budget, refund limit, SLA]
    F5[Select best policy, price, quantity, and routing]
  end

  %% ========= 7) Deployment and Feedback =========
  subgraph S7[Deployment and Monitoring]
    G1[Publish policy and price by region, store, channel]
    G2[Operational rules: return windows, labels, routing, fees]
    G3[Track KPIs: conversion, return rate, NPS, cost per return, margin]
    G4[Re-estimate inputs periodically]
    G5[Run experiments: A/B or multi-armed bandits]
  end

  %% ========= Edges =========
  S1 --> S2
  S2 --> S3
  S3 --> S4
  S4 --> S5
  S5 --> S6
  S6 -->|Best policy, p, q, routing| S7
  S7 -->|New data| S2

