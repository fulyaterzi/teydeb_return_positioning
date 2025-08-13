```mermaid

flowchart TD
  subgraph S1[Inputs and Configuration]
    A1[Historical sales and returns]
    A2[Channels Online or Store]
    A3[Regions and Stores]
    A4[Cost parameters unit cost c salvage s processing k_proc hassle h_store fees]
    A5[Policy set BORO BORS BSRS BSRO Partial refund r less than p]
    A6[Price grid P and Order Qty grid Q]
  end

  subgraph S2[Estimation and Feature Engineering]
    B1[Estimate market demand distribution F of x]
    B2[Estimate valuation distribution G of v]
    B3[Estimate return probability by policy channel region]
    B4[Service level and lead time inputs]
  end

  subgraph S3[Customer Utility and Demand Shaping]
    C1[Define customer utility U per policy and channel]
    C2[Online utility keep equals V minus p]
    C3[Online utility return equals expected V minus p plus refund r minus fee]
    C4[Store utility return equals V minus p minus hassle h_store plus refund r_store]
    C5[Compute adoption share alpha from G of v]
  end

  subgraph S4[Policy Conditional Demand]
    D1[Effective demand multiplier theta equals adoption share]
    D2[Demand under policy X_policy equals theta times base demand from F]
    D3[Expected sales equals expectation of min X_policy comma q]
    D4[Return rate rho by policy channel region]
  end

  subgraph S5[Profit and Feasibility]
    E1[Revenue kept equals p times one minus rho times expected sales]
    E2[Refunded or salvage equals s minus k_proc times rho times expected sales]
    E3[Unsold salvage equals s times q minus expected sales]
    E4[Cost equals negative c times q]
    E5[Logistics routing choose hub store or warehouse meeting cost and SLA]
    E6[Total profit equals sum of terms]
  end

  subgraph S6[Optimization Loop]
    F1[Enumerate policy options]
    F2[Search over price P and order quantity Q]
    F3[Compute profit for all regions stores channels]
    F4[Check constraints capacity budget refund limits SLA]
    F5[Select best policy price quantity and routing]
  end

  subgraph S7[Deployment and Monitoring]
    G1[Publish policy and price by region store channel]
    G2[Set operational rules return windows labels routing fees]
    G3[Track KPIs conversion return rate NPS cost per return margin]
    G4[Re estimate inputs periodically]
    G5[Run experiments A B tests or bandits]
  end

  S1 --> S2
  S2 --> S3
  S3 --> S4
  S4 --> S5
  S5 --> S6
  S6 -->|best policy price qty routing| S7
  S7 -->|new data| S2


