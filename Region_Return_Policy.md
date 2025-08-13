```mermaid
flowchart TD
    A["Start"] --> B["Inputs: region, customer city, price p, costs, courier availability, distances to stores. Assumption: full refund"]
    B --> C["Compute city specific hassle costs: h_store from distance to nearest store; h_online from city factors"]
    C --> D{"Store feasible in this city?"}
    D -- "Yes" --> E["Find nearest store within D_max and set return city"]
    D -- "No" --> F
    E --> G{"Online feasible in this region?"}
    D -- "No" --> G
    G -- "No" --> X["No feasible return path. Escalate to manual rule"]
    G -- "Yes" --> H["Evaluate expected profit for ONLINE and STORE using city hassle costs and full refund"]
    H --> I{"Is profit_online >= profit_store?"}
    I -- "Yes" --> O["Select ONLINE return policy"]
    I -- "No" --> S["Select STORE return policy with the chosen return city"]
    O --> Y["Output: Policy ONLINE; KPIs"]
    S --> Y
