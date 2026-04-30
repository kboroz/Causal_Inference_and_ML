# Woche 5 — Robuste Schätzung heterogener Treatment-Effekte

## Material im Repo
- **PDF:** `5_Heterogeneous Treatment Effects.pdf`
- **Notebooks (CausalAIBook, Modul PM2):**
  1. `python_linear_penalized_regs.ipynb`
  2. `python_ml_for_wage_prediction.ipynb`
- **Videos:** vier Videos (Playlist-Einträge 13–16).

## Was die Woche leisten soll
Die in Woche 4 eingeführten CATE-Schätzer **härten**: orthogonale Score-Funktionen, doubly-robuste Pseudo-Outcomes, Robustheit gegen Misspezifikation und unbeobachtete Confounder (Sensitivity).

## Kernkonzepte

### 1. Pseudo-Outcomes
Statt $Y$ direkt zu modellieren, konstruiert man ein **Pseudo-Outcome** $\psi_i$, dessen bedingter Erwartungswert genau $\tau(X_i)$ ist. Dann reduziert sich CATE-Schätzung auf Standard-Regression $\psi$ auf $X$.

**Doubly-Robust Pseudo-Outcome (AIPW):**
$$
\psi_i = \hat g(1, X_i) - \hat g(0, X_i) + \frac{T_i\,(Y_i - \hat g(1, X_i))}{\hat e(X_i)} - \frac{(1-T_i)\,(Y_i - \hat g(0, X_i))}{1-\hat e(X_i)}.
$$
$E[\psi \mid X] = \tau(X)$ unter Unconfoundedness, **doppelt robust** wie bei ATE.

### 2. R-Learner (Nie & Wager)
Zerlegt das Problem in Residual-Form (verallgemeinert FWL):
$$
\hat\tau = \arg\min_{\tau}\, \frac{1}{n}\sum_i \big((Y_i - \hat m(X_i)) - \tau(X_i)\,(T_i - \hat e(X_i))\big)^2 + \Lambda(\tau).
$$
Mit beliebigem $\tau$-Modell (Lasso, RF, NN) und beliebigem Regularisierer $\Lambda$. Erfüllt **Neyman-Orthogonalität** ⇒ Robust gegen Nuisance-Fehler.

### 3. DR-Learner
Zwei-Stufen-Verfahren:
1. Schätze $\hat g, \hat e$ mit Cross-Fitting.
2. Konstruiere $\psi_i$, regressiere $\psi$ auf $X$ mit beliebigem Lerner.

Praktisch sehr beliebt, einfach umsetzbar in `econml.DRLearner`.

### 4. Orthogonal Random Forests
Verallgemeinerung des Causal Forest: lokal orthogonalisierte Schätzung mit Lasso-Korrekturen — robust auch in High-Dimensional X.

### 5. Sensitivity Analysis
Was, wenn **Unconfoundedness** verletzt ist?
- **Rosenbaum-Bounds** (klassisch, Matching-basiert).
- **E-Value:** wie stark müsste ein unbeobachteter Confounder mit T und Y assoziiert sein, um den Effekt zu erklären?
- **Veitch & Zaveri / Cinelli & Hazlett:** moderne Sensitivity Analyses, die genau quantifizieren, wie viel „omitted variable bias" der gefundene Effekt verkraftet.

Faustregel: ein gefundener Effekt ist nur so glaubwürdig wie sein **schlechtester** plausibler Sensitivity-Test.

### 6. Penalisierte Regressionen vertieft (Notebook PM2)
- **Lasso, Ridge, Elastic Net** als Nuisance-Schätzer in DML.
- Cross-Validation für $\lambda$-Wahl mit Sample-Splitting, damit nichts leakt.
- Vergleich der Performance auf High-Dim-Wage-Daten.

## Fallstricke
- **„Robust" gegen Misspezifikation ≠ robust gegen unbeobachtete Confounder.** Doubly Robust setzt nach wie vor Unconfoundedness voraus.
- **Sensitivity Analysis weglassen** und Punktschätzung als „kausal" verkaufen — unseriös außerhalb von RCTs.
- **Hyperparameter über CATE-MSE tunen** ist tricky, weil $\tau$ nicht direkt beobachtbar ist (kein Ground Truth). Workarounds: R-Loss, DR-Score auf Test-Set.

## Self-Test
1. Schreibe das DR-Pseudo-Outcome auf und erkläre, warum sein bedingter Erwartungswert gleich $\tau(X)$ ist.
2. Was ist der Unterschied zwischen R-Learner und DR-Learner — wann welches?
3. Erkläre den E-Value an einem Satz: „Unser geschätzter Effekt RR=2.0 hat einen E-Value von 3.4." Was bedeutet das für die Glaubwürdigkeit?
4. Warum kann man CATE-Modelle nicht einfach per Test-MSE auf $Y$ vergleichen?

## Verbindung zu Woche 6
Woche 6 widmet sich genau diesem Modellauswahl-Problem: **Loss-Funktionen für kausale Inferenz** — wie misst man, ob ein CATE-Modell besser ist als ein anderes, wenn man die Wahrheit nicht sieht?
