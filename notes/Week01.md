# Woche 1 — Einführung

## Material im Repo
- **README** (Woche 1) verweist auf:
  - Tyler Vigens „Spurious Correlations" — https://www.tylervigen.com/spurious-correlations
  - YouTube-Playlist mit Einführungsvideos
  - Kaggle-Datensatz „CausalPitfalls Benchmark" (NeurIPS 2025) als möglicher Spielplatz für das Projekt
- **Aufgabe:** ein eigenes **Project Proposal** entwickeln, parallel zum Kurs.

## Was die Woche leisten soll
Den mentalen Umbau von „Korrelation" zu „Kausalität" anstoßen — und zwar so, dass es im Rest des Kurses **operationalisierbar** bleibt (mit Daten, Schätzern, Annahmen).

## Die fünf Kernkonzepte

### 1. Spurious Correlations
Zwei Zeitreihen können hochkorreliert sein, ohne dass eine die andere verursacht. Typische Ursachen:
- **Confounder** (gemeinsame Ursache),
- **Trend** (beide wachsen über die Zeit),
- **Selection** (wir schauen nur auf eine Teilpopulation),
- **Zufall** bei multiplem Testen.

### 2. Potential Outcomes (Rubin/Neyman)
Für jede Einheit $i$ existieren zwei Welten:
- $Y_i(1)$: Outcome **mit** Treatment,
- $Y_i(0)$: Outcome **ohne** Treatment.

Beobachtbar ist immer nur eine — das **fundamentale Problem der kausalen Inferenz**.

### 3. Treatment Effects
- **ATE**: $E[Y(1) - Y(0)]$ — Mittelwert über die Population.
- **ATT**: $E[Y(1) - Y(0) \mid T=1]$ — nur über die tatsächlich Behandelten.
- **CATE**: $\tau(x) = E[Y(1) - Y(0) \mid X=x]$ — heterogen, je nach Kovariaten. Hier kommt später ML ins Spiel.

### 4. Identifikationsannahmen
Ohne sie ist das Schätzen sinnlos:
- **SUTVA** — keine Spillover, eine Treatment-Version pro Einheit.
- **Unconfoundedness / Ignorability**: $(Y(0), Y(1)) \perp T \mid X$.
- **Overlap / Positivity**: $0 < P(T=1 \mid X) < 1$.
- **Consistency**: $Y = T \cdot Y(1) + (1-T) \cdot Y(0)$.

### 5. RCT vs. Observational
Im **RCT** ist $T$ unabhängig von allem — Annahmen geschenkt. In **Beobachtungsdaten** musst du sie *erkaufen*: durch Adjustment, Propensity-Scoring, IV, DiD, RDD … Die kommenden Wochen sind im Kern ein Werkzeugkasten dafür.

## Worauf in der Playlist achten
Pro Video eine Karteikarte mit drei Feldern:
1. **Problem** — Was geht schief?
2. **Annahme/Methode** — Was löst es?
3. **Bruchstelle** — Was passiert, wenn die Annahme verletzt ist?

Das Annahmen-Tracking ist der rote Faden des ganzen Kurses.

## Project Proposal (CausalPitfalls)
Der Kaggle-Benchmark ist ein sauberer Spielplatz mit eingebauten Fallstricken. Zwei Optionen:
- **(a)** Benchmark nutzen — sauber, vergleichbar, didaktisch wertvoll.
- **(b)** Eigene Daten (z. B. Energie / Verbrauch) — lehrt mehr, ist aber identifikationstechnisch anspruchsvoller.

Empfehlung: parallel beides explorieren, Entscheidung in Woche 3–4.

## Self-Test
1. Erkläre in einem Satz, warum $E[Y \mid T=1] - E[Y \mid T=0]$ in Beobachtungsdaten **kein** ATE ist.
2. Gib ein Beispiel, in dem **Overlap** verletzt ist — was bedeutet das praktisch?
3. Warum hilft Randomisierung gegen **unbeobachtete** Confounder, Adjustment aber nicht?

## Verbindung zu Woche 2
Woche 2 schlägt die Brücke: ML als Werkzeug für ökonometrische Schätzung — wie Prediction-Methoden in Estimation-Probleme eingebettet werden, ohne die statistische Inferenz zu zerstören.
