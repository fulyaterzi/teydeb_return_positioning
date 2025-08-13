```mermaid

flowchart TD
  A["Start"] --> B["Load inputs: customer, region, city; product; costs; capacities; distances(store, WH)"]

  B --> C{"Buy online?"}
  C -- "Yes" --> O0["Set purchase_channel = ONLINE"]
  C -- "No"  --> S0["Set purchase_channel = STORE"]

  %% ---- Choose optimal return policy (ONLINE vs STORE) ----
  O0 --> P1["Compute expected profit for return policy = ONLINE and STORE<br/>(demand, price, refund, fees, ops costs, expected returns, CSAT)"]
  S0 --> P2["Compute expected profit for return policy = ONLINE and STORE<br/>(demand, price, refund, fees, ops costs, expected returns, CSAT)"]

  P1 --> D1{"Argmax policy?"}
  P2 --> D2{"Argmax policy?"}

  D1 -- "ONLINE" --> R1
  D1 -- "STORE"  --> R2
  D2 -- "ONLINE" --> R1
  D2 -- "STORE"  --> R2

  %% ---- If chosen policy = ONLINE: route the carrier ----
  subgraph ONLINE_RETURN_ROUTING ["If chosen policy = ONLINE"]
    direction TB
    R1["Policy=ONLINE"] --> O1["Compute distances: d( pickup → WH ), d( pickup → hub_store )"]
    O1 --> O2{"Feasible capacity & SLA?"}
    O2 -- "Both feasible" --> O3{"d to WH ≤ d to hub_store?"}
    O3 -- "Yes" --> OWH["Route carrier → Warehouse (WH)"]
    O3 -- "No"  --> OHUB["Route carrier → Hub Store"]
    O2 -- "Only WH feasible"  --> OWH
    O2 -- "Only hub feasible" --> OHUB
    O2 -- "Neither" --> OX["Escalate (manual rule / overflow WH)"]
  end

  %% ---- If chosen policy = STORE: pick return store for customer ----
  subgraph STORE_RETURN_ROUTING ["If chosen policy = STORE"]
    direction TB
    R2["Policy=STORE"] --> S1["Find nearest store(s) to customer"]
    S1 --> S2{"Has capacity & within max distance?"}
    S2 -- "Yes" --> SOK["Assign nearest feasible store"]
    S2 -- "No"  --> S3["Try next-nearest / sibling stores"]
    S3 --> S4{"Found feasible store?"}
    S4 -- "Yes" --> SOK
    S4 -- "No"  --> SX["Fallback: allow mail-in / overflow WH / escalate"]
  end

  %% ---- Outputs ----
  OWH --> Y["OUTPUT: {purchase_channel, policy, return_destination, KPIs}"]
  OHUB --> Y
  OX --> Y
  SOK --> Y
  SX --> Y
