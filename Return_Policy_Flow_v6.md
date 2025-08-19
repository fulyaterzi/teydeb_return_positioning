```mermaid
%%{init: {'flowchart': {'htmlLabels': true}} }%%
flowchart TD
  A["<div style='width:300px; text-align:middle; padding:5px'>
      <b>Başlangıç / Girdiler</b><br/>
      kanal, <b>m = (p - c)</b>, r, h, &theta;, &lambda;, c<sub>carr</sub>
  </div>"]
    --> B["<div style='width:320px; text-align:middle; padding:5px'>
      <b>Eşiği hesapla</b><br/>
      T = -ln(1 - (&theta; · c<sub>carr</sub>)/m) / (&theta; · &lambda;)<br/>
      Eğer &theta; · c<sub>carr</sub> &ge; m ise T = &infin;
  </div>"]

  B --> C["<div style='padding:6px'>T = &infin; ?</div>"]

  %% --- T = ∞ (RO domine) ---
  C -- Evet --> D{r >= h ?}
  D -- Evet -->  RS1[MOD = RS]
  D -- Hayir --> NR1[MOD = NR]

  %% --- T sonlu ---
  C -- Hayir --> E{r >= T ?}
  E -- Evet --> F{h >= T ?}
  F -- Evet -->  RO[MOD = RO]
  F -- Hayir --> RS2[MOD = RS]

  E -- Hayir --> G{h >= T ?}
  G -- Evet -->  NR2[MOD = NR]
  G -- Hayir --> H{r >= h ?}
  H -- Evet -->  RS3[MOD = RS]
  H -- Hayir --> NR3[MOD = NR]

  %% --- Kanal bazlı politika eşleme (ara düğüm kullanıldı; kayma yok) ---
  subgraph ESLEME
    direction TB
    I["<div style='padding:6px'>Satış kanalı?</div>"]

    I --> OTXT["<div style='padding:4px'>Online</div>"]
    OTXT --> JO["<div style='width:300px; text-align:middle; padding:6px'>
      Karar: MOD=RO &rarr; BORO,<br/>
      MOD=RS &rarr; BORS, MOD=NR &rarr; BONR
    </div>"]

    I --> STXT["<div style='padding:4px'>Mağaza</div>"]
    STXT --> JS["<div style='width:300px; text-align:middle; padding:6px'>
      Karar: MOD=RO &rarr; BSRO,<br/>
      MOD=RS &rarr; BSRS, MOD=NR &rarr; BSNR
    </div>"]
  end

  RS1 --> I
  NR1 --> I
  RO  --> I
  RS2 --> I
  NR2 --> I
  RS3 --> I
  NR3 --> I
