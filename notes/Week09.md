# Woche 9 — Granger & Time-Series-Kausalität

## Material im Repo
- **Artikel-Serie „Causality in Data Science"** (Medium): warum ML Kausalität braucht, Causal Graphs, Confounding/Colliding/d-Separation, Annahmen für Discovery, Conditional Independence, SCMs, Causal Discovery in Multivariate-Time-Series, Hands-on Discovery, Hands-on Effect Estimation, nichtlineare Schätzung.
- **Notebooks / Tutorials:**
  - „Fun with ARMA, VAR and Granger Causality"
  - „Identifying Leading Indicators using Granger Causality"
  - GitHub: `cstorm125/granger/blob/master/trend_granger.ipynb`
  - „Forecasting with Granger Causality — Spurious Correlations" (TDS)

## Was die Woche leisten soll
Den Sprung in die Zeitdimension machen — und eine zentrale **Falle** früh markieren: **Granger-Kausalität ist Prädiktivität, nicht Kausalität**. Trotzdem nützlich, wenn man weiß, was sie misst.

## Kernkonzepte

### 1. Granger-Kausalität — Definition und Grenzen
$X$ Granger-causes $Y$, wenn:
$$
\text{Var}(Y_t \mid \mathcal F_{t-1}^{Y}) > \text{Var}(Y_t \mid \mathcal F_{t-1}^{Y, X}),
$$
also wenn $X$-Vergangenheit über die $Y$-Vergangenheit hinaus zur Vorhersage von $Y_t$ beiträgt.

Operational meist als **F-Test** in einem **VAR**:
$$
Y_t = \sum_{l=1}^p a_l Y_{t-l} + \sum_{l=1}^p b_l X_{t-l} + \varepsilon_t.
$$
$H_0$: alle $b_l = 0$.

**Wichtig:** Granger ist **prädiktive** Kausalität. Sie schließt unbeobachtete gemeinsame Treiber, Reverse Causation auf längeren Lags, und nicht-lineare Mechanismen nicht aus.

### 2. Stationarität, ADF, Differenzierung
Granger-Tests setzen typischerweise **Stationarität** voraus. Workflow:
- ADF / KPSS-Tests auf jede Reihe.
- Bei Nichtstationarität: differenzieren ($Y_t - Y_{t-1}$) oder Cointegrations-Setup nutzen.
- **Cointegration:** zwei nicht-stationäre Reihen, deren Linearkombination stationär ist — VECM-Modell statt VAR auf Differenzen.

### 3. Vector Autoregression (VAR)
$$
\mathbf Y_t = \mathbf c + \sum_{l=1}^p \mathbf A_l \mathbf Y_{t-l} + \boldsymbol\varepsilon_t.
$$
- Lag-Order $p$ via AIC/BIC/HQ wählen.
- Diagnostik: Residuals weiß? Stabilität (Eigenwerte innerhalb des Einheitskreises)?
- **Impulse Response Functions (IRF)** und **Forecast Error Variance Decomposition (FEVD)** zur Effektinterpretation.

### 4. Strukturelle vs. reduzierte Form
Reduzierte VAR ⇒ Granger-Tests, Vorhersagen.
Strukturelle VAR (SVAR) ⇒ kausale IRFs, aber nur unter **Identifikationsannahmen** (Cholesky-Reihenfolge, langfristige Restriktionen, Sign Restrictions).

### 5. Granger Causal Networks
Anwendung auf viele Reihen: paarweise Granger-Tests + Korrekturen ergeben gerichtete Netze (z. B. Hirnregionen, Finanzmärkte, Industriedaten). Vorsicht: bei vielen Reihen Multiple-Testing-Korrekturen unverzichtbar.

### 6. Nichtlineare und ML-basierte Erweiterungen
- **Transfer Entropy** als nicht-parametrische Verallgemeinerung von Granger.
- **Neural Granger Causality** (Tank, Covert, Foti & Fox 2018+): RNN/MLP mit gruppen-sparsen Eingangs-Gewichten — lernt Granger-Beziehungen ohne Linearitätsannahme.
- **PCMCI / PCMCI+ (Runge):** kombiniert PC-Algorithmus mit Conditional-Independence-Tests speziell für Time Series, kontrolliert Confounding über Lags.

### 7. Granger vs. Pearl
- **Granger:** prädiktiv, lineare Vergangenheit, keine Interventionen.
- **Pearl/SCM:** strukturell, do-Operator, Interventionen.
Beide nützlich, aber **nicht** austauschbar. Wenn die Frage „Was passiert, wenn ich eingreife?" lautet — Pearl. Wenn „Welche Reihe enthält Vorhersageinformation für die andere?" — Granger.

## Fallstricke
- **Granger-Aussage als Kausalaussage verkaufen.** Klassiker bei „X führt Y" in Schlagzeilen.
- **Nicht-Stationarität ignorieren** ⇒ Spurious Granger.
- **Zu kleine Lag-Order** ⇒ weggemittelte Effekte; **zu große** ⇒ Power-Verlust.
- **Aggregation über Zeit** kann Kausalrichtung umkehren oder verstecken (Aggregations-Bias).
- **Multiple Testing** in großen Granger-Netzen ohne FDR-Kontrolle.

## Self-Test
1. Erkläre an einem Energie-Beispiel den Unterschied zwischen „Wetter Granger-causet Verbrauch" und „Wetter verursacht Verbrauch (kausal)".
2. Wann brauchst du ein VECM statt eines VAR?
3. Warum ist eine SVAR-Identifikation nötig, um Impulse Response Functions kausal zu lesen?
4. Skizziere, wie PCMCI sich von paarweisen Granger-Tests unterscheidet.

## Verbindung zu Woche 10
Woche 10 ist **Project Session** — du präsentierst den aktuellen Stand. Bis dahin sollten Identifikationsstrategie, Daten, Methode und erste Schätzungen stehen. Diese Notes dienen als persönliches Cheat-Sheet, gegen das du dein Projekt prüfen kannst.
