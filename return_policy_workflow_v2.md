```mermaid

flowchart TD
  %% ---------- Inputs ----------
  A["Start"] --> B["Load inputs:<br/>customer, region, city<br/>price & refund rules<br/>store/WH locations & capacities<br/>costs (ops, carrier), SLAs, CSAT weights"]

  %% ---------- Purchase channel ----------
  B --> C{"Will the customer buy ONLINE?"}
  C -- "Yes" --> PC_ON["purchase_channel = ONLINE"]
  C -- "No"  --> PC_ST["purchase_channel = STORE"]

  %% ---------- Choose optimal return policy (includes NO_RETURN) ----------
  PC_ON --> P1["Evaluate expected profit/utility for policies:<br/>• ONLINE_RETURN<br/>• STORE_RETURN<br/>• NO_RETURN"]
  PC_ST --> P2["Evaluate expected profit/utility for policies:<br/>• ONLINE_RETURN<br/>• STORE_RETURN<br/>• NO_RETURN"]

  P1 --> D{"Argmax policy"}
  P2 --> D

  %% ---------- Branch by chosen policy ----------
  D -- "ONLINE_RETURN" --> OR0
  D -- "STORE_RETURN"  --> SR0
  D -- "NO_RETURN"     --> NR0["Set policy = NO_RETURN<br/>Return not allowed / discouraged"]

  %% ---------- ONLINE return routing (carrier) ----------
  subgraph ONLINE_RETURN_ROUTING ["If policy = ONLINE_RETURN"]
    direction TB
    OR0["Policy = ONLINE_RETURN"] --> OR1["Compute distances from pickup:<br/>d→WH, d→HubStore<br/>Check capacity/SLA for each"]
    OR1 --> OR2{"Feasible options?"}
    OR2 -- "Both feasible" --> OR3{"Is d→WH ≤ d→HubStore?"}
    OR3 -- "Yes" --> OR_WH["Route carrier → Warehouse (WH)"]
    OR3 -- "No"  --> OR_HUB["Route carrier → Hub Store"]
    OR2 -- "Only WH feasible"  --> OR_WH
    OR2 -- "Only Hub feasible" --> OR_HUB
    OR2 -- "Neither feasible"  --> OR_ESC["Escalate / overflow WH / manual rule"]
  end

  %% ---------- STORE return routing (customer brings to store) ----------
  subgraph STORE_RETURN_ROUTING ["If policy = STORE_RETURN"]
    direction TB
    SR0["Policy = STORE_RETURN"] --> SR1["Rank nearest stores to customer"]
    SR1 --> SR2{"Within max distance<br/>and capacity/SLA OK?"}
    SR2 -- "Yes" --> SR_OK["Assign that store"]
    SR2 -- "No"  --> SR3["Try next-nearest / sibling stores"]
    SR3 --> SR4{"Feasible store found?"}
    SR4 -- "Yes" --> SR_OK
    SR4 -- "No"  --> SR_ESC["Fallback: allow mail-in / overflow WH / manual rule"]
  end

  %% ---------- Unified outputs (consolidated last step) ----------
  OR_WH --> Z["OUTPUT:<br/>{purchase_channel, policy, return_destination, notes/KPIs}"]
  OR_HUB --> Z
  OR_ESC --> Z
  SR_OK  --> Z
  SR_ESC --> Z
  NR0    --> Z
