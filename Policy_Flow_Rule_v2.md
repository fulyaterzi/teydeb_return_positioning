```mermaid
flowchart TD
  A[Başlangıç / Girdiler: kanal, m, r, h, theta, lambda, c_carr]
    --> B["Threshold hesapla: T = -ln(1 - theta*c_carr/m)/(theta*lambda); eğer theta*c_carr >= m ise T = sonsuz"]
  B --> C{T sonlu mu?}

  %% --- T = sonsuz (RO domine) ---
  C -->|"Hayır (sonsuz)"| D{r >= h ?}
  D -->|"Evet"|  RS1[MOD = RS]
  D -->|"Hayır"| NR1[MOD = NR]

  %% --- T sonlu ---
  C -->|"Evet"| E{r >= T ?}
  E -->|"Evet"| F{h >= T ?}
  F -->|"Evet"|  RO[MOD = RO]
  F -->|"Hayır"| RS2[MOD = RS]

  E -->|"Hayır"| G{h >= T ?}
  G -->|"Evet"|  NR2[MOD = NR]
  G -->|"Hayır"| H{r >= h ?}
  H -->|"Evet"|  RS3[MOD = RS]
  H -->|"Hayır"| NR3[MOD = NR]

  %% --- Kanal bazlı politika eşleme ---
  subgraph EŞLEME
    direction TB
    I{Satış kanalı?}
    I -->|"Online"| JO[Politika: MOD=RO->BORO, MOD=RS->BORS, MOD=NR->BONR]
    I -->|"Mağaza"| JS[Politika: MOD=RO->BSRO, MOD=RS->BSRS, MOD=NR->BSNR]
  end

  RS1 --> I
  NR1 --> I
  RO  --> I
  RS2 --> I
  NR2 --> I
  RS3 --> I
  NR3 --> I

  %% --- Ölçekli bracket (bağıl kârlılık) ve override ---
  I --> K[Bracket kıyası: mx = max(0, m - theta*c_carr); valRO = exp(r*theta*lambda)*mx; valRS = exp((r - h)*theta*lambda)*m; valNR = m; kanala göre argmaksı seç]
  K --> L{Argmaks != eşlenen politika mı?}
  L -->|Hayır| M[Çıktı: eşlenen politika]
  L -->|Evet|  N[Çıktı: argmaks politika]
