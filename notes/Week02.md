# Woche 2 — Machine Learning in der Ökonometrie

## Material im Repo
- **Slides:**
  - `1.0 Machine Learning Econometrics`
  - `2.a Applied Machine Learning Intro`
  - `2.b Applied Machine Learning Secret Sauce`
  - `2.c Applied Machine Learning Prediction Estimation`
  - `Week02_CausalML_Slides.pdf`
  - `Week02_Vocabulary.pdf`
- **Notebook:** `Week02.ipynb` (im Repo, eigenständig)
- **Videos:** vier Videos aus der Stanford-GSB-Playlist (Einträge 2–5).

## Was die Woche leisten soll
Die Brücke zwischen klassischer Ökonometrie und ML schlagen — und klar machen, warum „guter Predictor" nicht automatisch „guter Estimator" ist.

## Kernkonzepte

### 1. Prediction vs. Estimation
- **Prediction**: Ziel ist möglichst kleiner Out-of-Sample-Loss von $\hat{Y}$. Erfolg = niedriger Test-MSE.
- **Estimation**: Ziel ist eine **konsistente, unverzerrte, asymptotisch normale** Schätzung eines **Parameters** $\theta$ (z. B. eines Treatment-Effekts) — mit gültigen Konfidenzintervallen.

ML ist auf das erste optimiert; klassische Ökonometrie auf das zweite. Causal ML versucht, beides zu kombinieren.

### 2. Bias-Variance-Tradeoff & Regularisierung
ML-Modelle (Lasso, Ridge, Trees, Boosting, NN) introducieren **Bias** zugunsten kleinerer **Variance**. Für Vorhersage gut, für Inferenz oft schlecht: regulierte Schätzer haben **nicht-zentrale Asymptotik** (Bias verschwindet nicht im $\sqrt{n}$-Sinne).

### 3. „Secret Sauce" — Sample Splitting & Cross-Fitting
Wenn man ML-Predictions in Schätzformeln einsetzt, entsteht **Overfitting-Bias**. Lösung:
- **Sample Splitting:** ML auf Hälfte 1 trainieren, Schätzung auf Hälfte 2.
- **Cross-Fitting:** $K$-Fold-Variante davon — nutzt alle Daten, gleiche Bias-Eigenschaften.

Das ist die zentrale Technik, die Double ML (Woche 3+) erst möglich macht.

### 4. Frisch-Waugh-Lovell (FWL) Theorem
Klassisches OLS-Resultat, das den ganzen Double-ML-Ansatz vorbereitet:
Um $\beta$ in $Y = X\beta + W\gamma + \varepsilon$ zu schätzen, kann man:
1. $Y$ auf $W$ regressieren → Residuum $\tilde{Y}$,
2. $X$ auf $W$ regressieren → Residuum $\tilde{X}$,
3. $\tilde{Y}$ auf $\tilde{X}$ regressieren → liefert dasselbe $\hat\beta$.

**Idee:** „Partialliere $W$ aus $Y$ und $X$ heraus." Wenn man Schritte 1 und 2 mit ML statt OLS macht, hat man Double ML.

### 5. Lasso & high-dimensional Regression
Wenn $p \gg n$ oder $p$ zumindest groß ist:
- **Lasso**: $\hat\beta = \arg\min \|Y - X\beta\|^2 + \lambda \|\beta\|_1$. Wählt Variablen aus (Sparsity).
- **Ridge**: $\ell_2$-Strafe, schrumpft alle Koeffizienten.
- **Elastic Net**: Kombi aus beidem.
Wichtig: Lasso-Koeffizienten direkt interpretieren ist gefährlich (post-selection bias). Deshalb **Double Lasso / Debiased Lasso** in Woche 3.

## Vocabulary (siehe `Week02_Vocabulary.pdf`)
Stichworte, die ab jetzt durchlaufen: Loss, Risk, Empirical Risk, Regularisierung, Hyperparameter, Cross-Validation, Train/Test/Split, Overfitting, Generalization Gap, Sparsity, Bias-Variance.

## Fallstricke
- **Hyperparameter-Tuning auf Test-Set** — leakt und verzerrt jede nachgelagerte Inferenz.
- **Lasso-Koeffizient = Effekt** — nein, post-selection inference ist eigenes Thema.
- **Ohne Sample Splitting** ML-Residuen in eine zweite Regression stecken — Standardfehler werden falsch.

## Self-Test
1. Beschreibe den FWL-Algorithmus in drei Schritten und erkläre, warum Schritt 3 dasselbe $\hat\beta$ liefert wie eine direkte Multiple-Regression.
2. Warum hat ein Lasso-Koeffizient typischerweise **keine** Standard-Normalverteilung um den wahren Wert?
3. Was genau löst Cross-Fitting, das einfaches Sample-Splitting nicht löst?

## Verbindung zu Woche 3
Woche 3 nutzt FWL + ML + Cross-Fitting → **Double / Debiased ML** zur Schätzung des **ATE** unter Confounding.
