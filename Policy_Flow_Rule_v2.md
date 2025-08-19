```mermaid
flowchart TD
  A[Başlangıç / Girdiler: kanal, m, r, h, theta, lambda, c_carr]
    --> B[Eşiği hesapla: T = -ln(1 - theta*c_carr/m)/(theta*lambda); eğer theta*c_carr >= m ise T = sonsuz]
  B --> C{T sonlu mu?}

  %% --- T = sonsuz (RO domine) ---
  C -->|Hayir (sonsuz)| D{r >= h ?}
  D -->|Evet|  RS1[MOD = RS]
  D -->|Hayir| NR1[MOD = NR]

  %% --- T sonlu ---
  C -->|Evet| E{r >= T ?}
  E -->|Evet| F{h >= T ?}
  F -->|Evet|  RO[MOD = RO]
  F -->|Hayir| RS2[MOD = RS]

  E -->|Hayir| G{h >= T ?}
  G -->|Evet|  NR2[MOD = NR]
  G -->|Hayir| H{r >= h ?}
  H -->|Evet|  RS3[MOD = RS]
  H -->|Hayir| NR3[MOD = NR]

  %% --- Kanal bazlı politika eşleme ---
  subgraph ESLEME
    direction TB
    I{Satış kanalı?}
    I -->|Online| JO[Politika: MOD=RO->BORO, MOD=RS->BORS, MOD=NR->BONR]
    I -->|Mağaza| JS[Politika: MOD=RO->BSRO, MOD=RS->BSRS, MOD=NR->BSNR]
  end

  RS1 --> I
  NR1 --> I
  RO  --> I
  RS2 --> I
  NR2 --> I
  RS3 --> I
  NR3 --> I


