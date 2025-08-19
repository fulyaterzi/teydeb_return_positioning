```mermaid

flowchart TD
    A[Başlangıç<br/>Girdiler: kanal, m, r, h, θ, λ, c_carr] --> B[ Eşiği hesapla:<br/>T = -ln(1 - θ·c_carr / m) / (θ·λ)<br/>(θ·c_carr ≥ m ⇒ T = ∞) ]
    B --> C{T sonlu mu?}

    %% --- T = ∞ (RO domine) kolu ---
    C -- "Hayır (T = ∞)" --> D{r ≥ h ?}
    D -- "Evet" --> E[MOD = RS]
    D -- "Hayır" --> F[MOD = NR]

    %% --- T sonlu kolu ---
    C -- "Evet" --> G{r ≥ T ?}
    G -- "Evet" --> H{h ≥ T ?}
    H -- "Evet" --> I[MOD = RO]
    H -- "Hayır" --> J[MOD = RS]

    G -- "Hayır (r < T)" --> K{h ≥ T ?}
    K -- "Evet" --> L[MOD = NR]
    K -- "Hayır" --> M{r ≥ h ?}
    M -- "Evet" --> N[MOD = RS]
    M -- "Hayır" --> O[MOD = NR]

    %% --- Kanala göre politika eşleme ---
    E --> P
    F --> P
    I --> P
    J --> P
    L --> P
    N --> P
    O --> P
    P[Kanala göre politika eşleme:<br/>Online: RO→BORO, RS→BORS, NR→BONR<br/>Mağaza: RO→BSRO, RS→BSRS, NR→BSNR]

    %% --- Ölçekli bracket ve argmaks ---
    P --> Q[Ölçekli 'bracket' değerleri:<br/>m_x = max(0, m − θ·c_carr)<br/>val_RO = e^{rθλ} · m_x<br/>val_RS = e^{(r−h)θλ} · m<br/>val_NR = m<br/>Kanal kümesine göre argmaksı seç:<br/>Online {BORO,BORS,BONR}<br/>Mağaza {BSRO,BSRS,BSNR}]
    Q --> R{Argmaks ≠ eşlenen politika mı?}
    R -- "Hayır" --> S[Sonuç: Eşlenen politika]
    R -- "Evet" --> T[Sonuç: Argmaks politika]
