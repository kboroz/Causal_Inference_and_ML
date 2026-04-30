# Woche 6 — Loss-Funktionen für kausale Inferenz

## Material im Repo
- **PDF:** `6_Loss_Functions.pdf`
- **Notebooks (CausalAIBook, Modul PM2):**
  1. `python_convergence_hypothesis_double_lasso.ipynb`
  2. `python_heterogeneous_wage_effects.ipynb`
- **Videos:** vier Videos (Playlist-Einträge 17–20).

## Was die Woche leisten soll
Das Modellauswahl-Problem für kausale Inferenz ernst nehmen. **Predictive MSE** lügt bei CATE — wir brauchen Surrogat-Losses, die Treatment-Effekt-Qualität messen, ohne den Ground Truth zu kennen.

## Kernkonzepte

### 1. Warum klassische Test-MSE versagt
Wir beobachten **nie** $\tau_i = Y_i(1) - Y_i(0)$. Also:
$$
\text{MSE}_\tau(\hat\tau) = E[(\tau(X) - \hat\tau(X))^2]
$$
ist **nicht direkt schätzbar**. Trotzdem brauchen wir ein Auswahlkriterium für Hyperparameter, Modellklassen, Lerner.

### 2. Surrogat-Losses

**a) R-Loss (Nie & Wager).**
Mit residualisierten Größen $\tilde Y = Y - \hat m(X)$, $\tilde T = T - \hat e(X)$:
$$
\hat L_R(\hat\tau) = \frac{1}{n}\sum_i \big(\tilde Y_i - \hat\tau(X_i)\,\tilde T_i\big)^2.
$$
Niedriger R-Loss = besseres CATE-Modell. Funktioniert, weil unter Orthogonalität gilt $E[L_R(\tau)] = E[(\tau(X) - \tilde\tau(X))^2 \cdot \mathrm{Var}(T\mid X)] + \text{const}$.

**b) DR-Loss / IF-basiertes Auswahlkriterium.**
Vergleich auf Basis des Doubly-Robust Pseudo-Outcomes $\psi$:
$$
\hat L_{DR}(\hat\tau) = \frac{1}{n}\sum_i (\psi_i - \hat\tau(X_i))^2.
$$
Funktioniert, weil $E[\psi\mid X] = \tau(X)$.

**c) Influence-Function-basierte Score (Alaa & van der Schaar).** Verwendet Effizienz-Theorie der Schätzfunktion direkt für Modellselektion.

### 3. Konvergenzraten und Double Lasso
Notebook `convergence_hypothesis_double_lasso` zeigt empirisch:
- Lasso allein konvergiert nicht $\sqrt n$ schnell auf den Effektkoeffizienten.
- Double Lasso (Belloni/Chernozhukov/Hansen) erreicht $\sqrt n$ — selbst in High-Dimension.
- Praktisch: Konfidenzintervalle sind nur valide, wenn der Schätzer **orthogonal + cross-fit** ist.

### 4. Heterogene Lohneffekte (Notebook)
Anwendungsbeispiel: nicht *ob* es einen Geschlechter-Lohngap gibt, sondern *wo* er groß ist (Bildung, Branche, Region). Klassischer Use Case für CATE-Schätzung + Loss-basierte Modellselektion.

### 5. Validierung in der Praxis
Empfohlener Workflow:
1. **Synthetische Validierung:** simuliere Daten mit bekanntem $\tau(x)$, vergleiche, welche Loss-Funktion am besten mit MSE_$\tau$ korreliert.
2. **Sample Splitting:** Train/Tune/Test getrennt; Loss auf separatem Hold-out berechnen.
3. **Mehrere Surrogate kombinieren:** R-Loss und DR-Loss übereinstimmend → mehr Vertrauen.

## Fallstricke
- **CV mit MSE auf $Y$** zur Auswahl eines CATE-Modells — wählt prädiktive, nicht kausale Modelle.
- **Surrogat-Loss ohne Cross-Fitting** der Nuisance — Loss selbst wird verzerrt.
- **Reine Punktschätzungen vergleichen** ohne Standardfehler des Loss — bei Rauschen sind Unterschiede zufällig.

## Self-Test
1. Warum kann man $\text{MSE}_\tau$ nicht direkt schätzen, $\text{MSE}_Y$ aber schon?
2. Was passiert mit $L_R(\hat\tau)$, wenn $\hat e$ und $\hat m$ schlecht sind — und warum heißt das „doubly robust" für Modellauswahl?
3. Skizziere ein synthetisches Experiment, mit dem du R-Loss vs. DR-Loss empirisch vergleichst.

## Verbindung zu Woche 7
Bisher: Effekte schätzen unter gegebenem Causal Graph. Ab Woche 7 dreht sich der Spieß um — wir lernen die **Graph-Struktur** selbst kennen: DAGs, Backdoor, do-Kalkül, Causal Discovery.
