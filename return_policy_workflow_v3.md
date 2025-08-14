```mermaid

flowchart TD
  %% ---------- Inputs ----------
  A["Start"] --> B["Load inputs"]

  %% ---------- Purchase channel ----------
  B --> C{"Buy ONLINE?"}
  C -- "Yes" --> PC_ON["purchase_channel=ONLINE"]
  C -- "No"  --> PC_ST["purchase_channel=STORE"]

  %% ---------- Choose policy (incl. NO_RETURN) ----------
  PC_ON --> P1["Evaluate policies"]
  PC_ST --> P2["Evaluate policies"]
  P1 --> D{"Best policy?"}
  P2 --> D

  %% ---------- Branch by chosen policy ----------
  D -- "ONLINE_RETURN" --> OR0
  D -- "STORE_RETURN"  --> SR0
  D -- "NO_RETURN"     --> NR0["policy=NO_RETURN"]

  %% ---------- ONLINE return routing (carrier) ----------
  subgraph ONLINE_RETURN_ROUTING ["If policy = ONLINE_RETURN"]
    direction TB
    OR0["ONLINE_RETURN"] --> OR1["Distances & SLA"]
    OR1 --> OR2{"Feasible?"}
    OR2 -- "Both" --> OR3{"WH ≤ Hub?"}
    OR3 -- "Yes" --> OR_WH["Route → WH"]
    OR3 -- "No"  --> OR_HUB["Route → HubStore"]
    OR2 -- "Only WH"  --> OR_WH
    OR2 -- "Only Hub" --> OR_HUB
    OR2 -- "None"     --> OR_ESC["Escalate"]
  end

  %% ---------- STORE return routing ----------
  subgraph STORE_RETURN_ROUTING ["If policy = STORE_RETURN"]
    direction TB
    SR0["STORE_RETURN"] --> SR1["Nearest stores"]
    SR1 --> SR2{"Meets limits?"}
    SR2 -- "Yes" --> SR_OK["Assign store"]
    SR2 -- "No"  --> SR3["Try next"]
    SR3 --> SR4{"Found?"}
    SR4 -- "Yes" --> SR_OK
    SR4 -- "No"  --> SR_ESC["Fallback/Escalate"]
  end

  %% ---------- Unified outputs (short label) ----------
  OR_WH --> Z["Output"]
  OR_HUB --> Z
  OR_ESC --> Z
  SR_OK  --> Z
  SR_ESC --> Z
  NR0    --> Z



  %% --- Invisible spacer for vertical alignment ---
  SPACER[" "]:::invisible

  %% --- Legend (unconnected) ---
  L["Legend:
  • 'Evaluate policies' = argmax of {ONLINE_RETURN, STORE_RETURN, NO_RETURN}
  • 'Distances & SLA' = distance + capacity + SLA checks
  • Output = {purchase_channel, policy, return_destination, KPIs/notes}
  "]:::legend

  %% Force vertical alignment by ordering
  FLOW --> SPACER
  SPACER --- L

  classDef legend fill:#f7f7f7,stroke:#bbb,color:#333,font-size:12px;
  classDef invisible fill:none,stroke:none,stroke-width:0px;



