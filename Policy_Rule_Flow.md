```mermaid


flowchart TD
    A[Baslangic / Inputs: channel, m, r, h, theta, lambda, c_carr]
      --> B["T'yi hesapla: T = -ln(1 - theta*c_carr/m)/(theta*lambda);  if theta*c_carr >= m then T = inf"]
    B --> C{T finite?}

    %% --- T = inf (RO dominated) ---
    C -- "No (inf)" --> D{r >= h?}
    D -- Yes --> E[MODE = RS]
    D -- No  --> F[MODE = NR]

    %% --- T finite ---
    C -- Yes --> G{r >= T?}
    G -- Yes --> H{h >= T?}
    H -- Yes --> I[MODE = RO]
    H -- No  --> J[MODE = RS]

    G -- No  --> K{h >= T?}
    K -- Yes --> L[MODE = NR]
    K -- No  --> M{r >= h?}
    M -- Yes --> N[MODE = RS]
    M -- No  --> O[MODE = NR]

    %% --- Kanal bazli politika isimleri ---
    E --> P
    F --> P
    I --> P
    J --> P
    L --> P
    N --> P
    O --> P
    P["Kanal esleme: Online -> BORO/BORS/BONR ; Store -> BSRO/BSRS/BSNR"]

    %% --- Scaled brackets ve override ---
    P --> Q["Scaled brackets:  m_x = max(0, m - theta*c_carr);  val_RO = exp(r*theta*lambda)*m_x;  val_RS = exp((r-h)*theta*lambda)*m;  val_NR = m;  kanala gore argmax sec"]
    Q --> R{Argmax != mapped?}
    R -- No  --> S[Sonuc: mapped policy]
    R -- Yes --> T[Sonuc: argmax policy]




