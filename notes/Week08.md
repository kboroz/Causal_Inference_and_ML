# Woche 8 — Netzwerk-Kausalität (Spillover, Peer Effects, DML in Nets)

## Material im Repo
- **Artikel:** *Granger Causal Networks and Indirect Feedback* (TDS).
- **Videos:** acht Videos (Playlist-Einträge 8–15 der zweiten Playlist) — vertiefen Discovery, Spillover, DAG-Inferenz.
- **Notebooks (CausalAIBook, Modul PM4):**
  1. `python_dml_inference_for_gun_ownership.ipynb` — DML auf einer policy-relevanten Frage, mit Netzwerk- bzw. spatial-strukturierten Confoundern.
  2. `python-identification-analysis-of-401-k-example-w-dags.ipynb` — Identifikationsanalyse am 401(k)-Klassiker, mit DAGs.

## Was die Woche leisten soll
SUTVA ist oft Wunschdenken: in sozialen Netzwerken, Märkten, Schulen, Stromnetzen wirkt das Treatment einer Einheit auf andere. Diese Woche zeigt, was schiefgeht, wenn man das ignoriert — und welche Modelle Spillover sauber quantifizieren.

## Kernkonzepte

### 1. SUTVA noch einmal — und ihre Verletzungen
SUTVA = **Stable Unit Treatment Value Assumption**: keine Spillovers, eine Treatment-Version. Verletzt durch:
- **Interferenz / Spillover:** Treatment von Nachbarn beeinflusst dein Outcome.
- **Mehrere Treatment-Versionen:** Variation, die wir als „T=1" zusammenwerfen.
- **General Equilibrium:** Skalierte Interventionen verändern Preise/Marktbedingungen → Effekt skaliert nicht linear.

### 2. Exposure-Mapping
Statt $T_i$ ist nun $E_i = f(T, A, i)$ relevant — die **Exposure** der Einheit, abhängig vom Adjazenzgraphen $A$. Beispiele:
- Anzahl behandelter Nachbarn.
- Anteil behandelter Nachbarn.
- Distanz zum nächsten behandelten Knoten.

Jede Exposure-Funktion ist eine **Annahme** über den Spillover-Mechanismus.

### 3. Direkte vs. indirekte Effekte
- **Direkter Effekt:** Mein Treatment auf mein Outcome bei festgehaltener Nachbar-Exposure.
- **Spillover-Effekt:** Nachbar-Treatment auf mein Outcome bei festgehaltenem eigenen Treatment.
- **Total Effekt:** Effekt einer global ausgerollten Politik.

In RCTs auf Netzwerken werden **Cluster-randomisierte** oder **two-stage designs** eingesetzt, um diese zu separieren.

### 4. DML auf hochdimensionalen Confounder-Strukturen
Notebook 1 (Gun Ownership) zeigt: in vielen empirischen Studien gibt es viele zeitlich/räumlich strukturierte Confounder (Demografie, Politik, Region). DML mit Cross-Fitting + flexibler Nuisance-Schätzung liefert valide Inferenz, ohne Pre-Selection von Kontrollen.

### 5. 401(k)-Beispiel mit DAG (Notebook 2)
Klassiker aus der Causal-ML-Literatur: Effekt von 401(k)-Eligibility auf Vermögensaufbau.
- DAG explizit machen → Adjustment-Set ableiten (Backdoor).
- DML / DR-Schätzer anwenden.
- Sensitivity prüfen.

Lehrwert: zeigt den vollständigen Workflow **DAG → Identifikation → Schätzung → Inferenz** an einem realen Datensatz.

### 6. Peer Effects: das Reflexionsproblem (Manski)
In linearen Peer-Modellen $Y_i = \alpha + \beta\,\bar Y_{-i} + \gamma\,\bar X_{-i} + \delta X_i + \varepsilon_i$ sind **Endogenous** ($\bar Y_{-i}$), **Exogenous/Contextual** ($\bar X_{-i}$) und **Correlated Effects** ohne weitere Annahmen **nicht trennbar**. Lösungen: Instrumente, partielle Identifikation, Variation in Netzwerk-Topologie.

## Fallstricke
- **Spillover ignorieren** ⇒ Direkteffekt + Bias unbestimmten Vorzeichens. Besonders gefährlich in dichten Netzen.
- **Falsches Exposure-Mapping** ⇒ Misspezifikation, die DML nicht heilt.
- **Cluster-Standardfehler vergessen**, wenn Einheiten in Clustern (Schulen, Haushalte, Regionen) korreliert sind.
- **Reflexionsproblem unterschätzen:** trennt man Peer-Influence nicht von Homophilie und gemeinsamem Schock, ist alles miteinander vermengt.

## Self-Test
1. Skizziere ein Beispiel aus Energiemanagement, in dem SUTVA verletzt ist — und gib eine plausible Exposure-Funktion an.
2. Was unterscheidet einen direkten von einem Spillover-Effekt mathematisch?
3. Warum reicht eine RCT auf Individual-Ebene **nicht** zur sauberen Schätzung von Total-Effekten in Netzwerken?
4. Erkläre Manskis Reflexionsproblem in zwei Sätzen.

## Verbindung zu Woche 9
Bisher querschnittlich oder netzwerk-strukturiert. Woche 9 nimmt die Zeitachse: **Granger-Kausalität** und Time-Series-Causality — und die scharfe Linie zwischen prädiktiver „Granger-Kausalität" und struktureller Kausalität.
