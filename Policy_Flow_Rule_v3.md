```mermaid
%%{init: {'flowchart': {'htmlLabels': true}} }%%
flowchart TD
  A[Baslangic / Girdiler: satis kanali, m, r, h, theta, lambda, c_carr] --> B[T yi hesapla; eger theta*c_carr >= m ise T sonsuz]
  B --> C{T sonlu mu?}

  %% --- T = sonsuz (RO domine) ---
  C -- Hayir --> D{r >= h ?}
  D -- Evet -->  RS1[MOD = RS]
  D -- Hayir --> NR1[MOD = NR]

  %% --- T sonlu ---
  C -- Evet --> E{r >= T ?}
  E -- Evet --> F{h >= T ?}
  F -- Evet -->  RO[MOD = RO]
  F -- Hayir --> RS2[MOD = RS]

  E -- Hayir --> G{h >= T ?}
  G -- Evet -->  NR2[MOD = NR]
  G -- Hayir --> H{r >= h ?}
  H -- Evet -->  RS3[MOD = RS]
  H -- Hayir --> NR3[MOD = NR]

  %% --- Kanal bazli politika esleme (ara dugum + genis kutu) ---
  subgraph ESLEME
    direction TB
    I{Satis kanali?}

    I --> OTXT[Online]
    OTXT --> JO["<div style='width:300px; text-align:left; padding:6px'>
    Politika: MOD=RO-&gt;BORO,<br/>MOD=RS-&gt;BORS, MOD=NR-&gt;BONR
    </div>"]

    I --> STXT[Magaza]
    STXT --> JS["<div style='width:300px; text-align:left; padding:6px'>
    Politika: MOD=RO-&gt;BSRO,<br/>MOD=RS-&gt;BSRS, MOD=NR-&gt;BSNR
    </div>"]
  end

  RS1 --> I
  NR1 --> I
  RO  --> I
  RS2 --> I
  NR2 --> I
  RS3 --> I
  NR3 --> I
