# Woche 3 — ML zur Schätzung von Average Treatment Effects (ATE)

## Material im Repo
- **Slides:**
  - `3.a ATE Intro`
  - `3.b ATE Confounding`
  - `3.c ATE Propensity Scores`
  - `3.d ATE Double Robustness`
  - `Week03_CausalML_Week03.pdf`, `Week03_Vocabulary.pdf`
- **Notebooks (Colab/CausalAIBook):**
  1. `python-ols-and-lasso-for-wage-prediction.ipynb` (Lena ✅)
  2. `python-ols-and-lasso-for-wage-gap-inference.ipynb`
- **Videos:** vier Videos (Playlist-Einträge 6–9).

## Was die Woche leisten soll
Vom Konzept des ATE zum **operationalen Schätzer** unter Beobachtungsdaten — und zwar so, dass er **valide Konfidenzintervalle** liefert, auch wenn Nuisance-Funktionen mit ML geschätzt werden.

## Kernkonzepte

### 1. Confounding formalisieren
$Y = g(T, X) + \varepsilon$, $T = m(X) + \eta$. Wenn $X$ sowohl $T$ als auch $Y$ beeinflusst, ist die naive Differenz biased:
$$
E[Y\mid T=1] - E[Y\mid T=0] \neq \text{ATE}.
$$
Adjustieren über $X$ ist die Standardstrategie unter **Unconfoundedness**.

### 2. Drei klassische ATE-Schätzer

**a) Outcome Regression (G-Computation):**
$$
\widehat{\text{ATE}} = \frac{1}{n}\sum_i \big[\hat{g}(1, X_i) - \hat{g}(0, X_i)\big].
$$
Konsistent, wenn $\hat g$ konsistent.

**b) Inverse Propensity Weighting (IPW):**
Mit Propensity Score $e(x) = P(T=1\mid X=x)$:
$$
\widehat{\text{ATE}} = \frac{1}{n}\sum_i \left[\frac{T_i Y_i}{\hat e(X_i)} - \frac{(1-T_i)Y_i}{1-\hat e(X_i)}\right].
$$
Konsistent, wenn $\hat e$ konsistent. Sehr empfindlich gegen Overlap-Verletzungen.

**c) Doubly Robust / AIPW:**
$$
\widehat{\text{ATE}} = \frac{1}{n}\sum_i \Big[\hat g(1,X_i) - \hat g(0,X_i) + \frac{T_i(Y_i - \hat g(1,X_i))}{\hat e(X_i)} - \frac{(1-T_i)(Y_i - \hat g(0,X_i))}{1-\hat e(X_i)}\Big].
$$
**„Doubly robust"**: konsistent, wenn **entweder** $\hat g$ **oder** $\hat e$ konsistent ist (nicht beide).

### 3. Propensity Score
$e(x) = P(T=1 \mid X=x)$. Anwendungen:
- **Matching** (gleiche Propensity Scores paaren),
- **Stratification** (in Buckets),
- **Weighting** (IPW),
- **Trimming** (extreme Propensities werfen).

Schätzbar mit Logit, aber auch mit Random Forest / Gradient Boosting / Neural Net.

### 4. Double / Debiased ML (DML)
Kombiniert FWL + ML + Cross-Fitting:
1. Auf Fold A: schätze $\hat g$ und $\hat m = \hat e$.
2. Auf Fold B: bilde Residuen $\tilde Y = Y - \hat g(X)$ und $\tilde T = T - \hat e(X)$.
3. Regressiere $\tilde Y$ auf $\tilde T$ → $\hat\theta$ ist konsistent und **asymptotisch normal mit $\sqrt n$-Rate**, auch wenn $\hat g, \hat e$ langsamer konvergieren ($n^{1/4}$ reicht).

Schlüssel-Eigenschaft: **Neyman-Orthogonalität** der Score-Funktion — kleine Fehler in den Nuisance-Schätzern haben Effekt zweiter Ordnung auf $\hat\theta$.

### 5. Wage-Gap-Beispiel (Notebooks)
Klassisches Anwendungsbeispiel: Unterschied im Lohn zwischen Männern und Frauen unter Kontrolle für Education, Experience, Occupation … Lasso wählt Kontrollen aus, Double Lasso liefert valide Inferenz auf den Geschlechter-Koeffizienten.

## Fallstricke
- **Overlap-Verletzung:** Propensities nahe 0 oder 1 lassen IPW explodieren. Immer Verteilung von $\hat e$ plotten.
- **Single sample (kein Cross-Fitting):** Standardfehler unterschätzt.
- **„Doubly robust" ≠ doppelte Sicherheit gegen Misspezifikation:** beide stark falsch ⇒ trotzdem biased.
- **Kontrollen, die selbst vom Treatment beeinflusst werden** („Bad Controls", Mediator/Collider) — verzerren das Ergebnis.

## Self-Test
1. Warum erfordert IPW Overlap, Outcome Regression aber „nur" korrekte Spezifikation von $g$?
2. Skizziere den DML-Algorithmus in fünf Zeilen Pseudocode.
3. Was bedeutet **Neyman-Orthogonalität** intuitiv — und warum ist sie der Grund, dass langsame ML-Raten ($n^{1/4}$) reichen?

## Verbindung zu Woche 4
Bisher: ein einziger ATE-Wert. Woche 4 erweitert auf **CATE** — die Heterogenität, *für wen* der Effekt groß oder klein ist — mit Causal Trees & Forests.
