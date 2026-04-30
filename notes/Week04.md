# Woche 4 — ML für CATEs: Causal Trees & Forests

## Material im Repo
- **Slides:**
  - `4.a CATE Overview`
  - `4.b CATE Trees`
  - `4.c CATE Forests`
- **Notebooks (CausalAIBook, Modul CM1):**
  1. `python-rct-vaccines.ipynb` (Sajjad)
  2. `python-sim-precision-adj.ipynb` (Aadip)
  3. `python-rct-penn-precision-adj.ipynb` (Ali)
- **Videos:** drei Videos (Playlist-Einträge 10–12).

## Was die Woche leisten soll
Von „durchschnittlichem Effekt" zu „Effekt für wen" — heterogene Treatment-Effekte schätzen, **ohne** in Overfitting-Inferenz-Fallen zu tappen.

## Kernkonzepte

### 1. CATE: Definition und Motivation
$$
\tau(x) = E[Y(1) - Y(0) \mid X = x].
$$
Politikrelevant: Wer profitiert (Targeting), wer schadet (Risikogruppen)? Im Marketing/Health/Energie ist CATE meist *die* eigentlich interessante Größe.

### 2. Meta-Learner als Einstieg
Bevor Causal Trees: drei einfache Strategien aus generischen ML-Modellen:
- **S-Learner**: ein Modell auf $(X, T) \to Y$. Gut bei kleinem Effekt; T wird schnell „weggeregelt".
- **T-Learner**: zwei Modelle, $\hat g_1$ (T=1) und $\hat g_0$ (T=0); $\hat\tau(x) = \hat g_1(x) - \hat g_0(x)$. Gut bei viel Daten in beiden Gruppen.
- **X-Learner**: T-Learner + Cross-Imputation + Propensity-Gewichtung. Gut bei unbalanciertem Treatment.
- **R-Learner / DR-Learner**: nutzen DML-Residualisierung bzw. Doubly-Robust-Pseudo-Outcomes.

### 3. Causal Trees (Athey & Imbens)
Klassische Bäume teilen Knoten so, dass **Outcome-Heterogenität** maximiert wird. Causal Trees teilen so, dass **Treatment-Effekt-Heterogenität** maximiert wird.

**Honest splitting:** Daten in zwei Hälften:
- Eine Hälfte zum Bestimmen der Splits,
- die andere zum Schätzen der Blattmittelwerte / Treatment-Effekte.

Dadurch sind Schätzungen **unverzerrt** im Blatt — Voraussetzung für valide Konfidenzintervalle.

### 4. Causal Forests (Wager & Athey)
- Ensemble vieler Causal Trees mit Subsampling und Random-Feature-Selection.
- **Asymptotische Normalität** der CATE-Schätzung pointweise.
- Implementierung: `grf` (R) ist die Referenzimplementierung; `econml` (Python) bietet `CausalForestDML`.

Geschätzt wird typischerweise das DML-Pseudo-Outcome:
$$
\tilde Y_i = (Y_i - \hat g(X_i)) - \tau(X_i)\,(T_i - \hat e(X_i)),
$$
und die lokale Lösung von $\tau(x)$ über die Forest-Gewichte.

### 5. Precision Adjustment in RCTs
Selbst in randomisierten Studien lohnt sich Adjustment auf Pre-Treatment-Kovariaten: nicht zur Bias-Reduktion (Randomisierung erledigt das), sondern für **kleinere Standardfehler** und damit höhere Power. Genau das illustrieren die drei Notebooks.

## Fallstricke
- **CATE-Schätzungen ohne Honest Splitting** sehen toll aus — sind aber overfit.
- **Heterogenität explorieren ohne Pre-Registrierung** → multiple Testing → Scheinmuster.
- **CATE pointwise schätzen, ohne Konfidenzintervalle zu zeigen** — Heterogenität-Visualisierungen ohne Unsicherheit sind irreführend.
- **„Bestes Subgroup"-Effekt rauspicken** ohne Korrektur — extrem optimistisch.

## Self-Test
1. Warum splittet ein Causal Tree anders als ein klassischer Regression Tree?
2. Wann verwendet man X-Learner statt T-Learner?
3. Wie liefert ein Causal Forest ein Konfidenzintervall für $\tau(x)$ — Stichwort „infinitesimal jackknife"?
4. Warum reduziert Adjustment in einem RCT die Varianz, ohne Bias einzuführen?

## Verbindung zu Woche 5
Woche 5 vertieft **robuste Schätzung von heterogenen Treatments** — dort kommen Doubly-Robust-Learner, Orthogonal Random Forests und Sensitivity Analyses.
