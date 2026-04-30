# Woche 10 — Project Session (Zwischenstand)

## Material im Repo
- **README:** „Present your current state of your project!"
- Keine Slides, kein Notebook — diese Woche ist **Arbeitsstand zeigen, Feedback einholen, Schwächen finden, bevor sie in der Final Presentation wehtun**.

## Was die Woche leisten soll
Ehrlicher Zwischenstand. Ziel ist nicht „fertig", sondern **„genug, um produktives Feedback zu bekommen"**. Erwartet wird typischerweise:
- klare Forschungsfrage,
- definierte Identifikationsstrategie,
- Daten exploriert,
- erste Schätzergebnisse mit Unsicherheit,
- bekannte offene Punkte.

## Vorbereitungs-Checkliste

### 1. Forschungsfrage in einem Satz
*„Welchen Effekt hat **[Treatment T]** auf **[Outcome Y]** bei **[Population/Setting]**?"*
Jede Folie der Präsentation sollte sich auf diesen Satz zurückführen lassen.

### 2. Identifikationsstrategie explizit machen
- Welche Annahmen brauchst du? (Unconfoundedness? Parallel Trends? Exclusion Restriction? SUTVA?)
- Welches Identifikationsdesign? (RCT, DML, IV, DiD, RDD, Synthetic Control, Causal Forest?)
- Warum dieses und nicht ein anderes?

### 3. DAG zeichnen (auch wenn klein)
Selbst eine grobe DAG mit fünf Knoten zwingt zur Klarheit über:
- Treatment, Outcome,
- vermutete Confounder,
- Mediatoren / Bad Controls,
- Selection-Mechanismen.

### 4. Daten-Realität
- Größe, Variablen, Zeitraum.
- Treatment-Definition (binär? kontinuierlich? Dose-Response?).
- Overlap-Diagnose: Histogramm der Propensity Scores.
- Fehlende Werte, Outliers, Cluster-Struktur.

### 5. Erste Schätzung — auch wenn vorläufig
Lieber **eine** Schätzung mit Konfidenzintervall und Annahmencheck als drei undokumentierte Punkte. Setup:
- Baseline (z. B. naiver OLS / einfache Mean-Diff).
- Hauptmethode (z. B. DML / Causal Forest).
- Vergleich der Ergebnisse — Differenz erklärt sich durch Confounding-Korrektur?

### 6. Robustness-Plan (auch wenn nur teilweise umgesetzt)
- Alternative Spezifikationen / Lerner.
- Sensitivity (E-Value, Cinelli-Hazlett).
- Placebo-Tests / Pre-Trends (bei DiD).
- Subgroup-Konsistenz.

### 7. Offene Punkte explizit benennen
*„Hier stecke ich gerade fest"* ist die wertvollste Folie der Session — dafür ist sie da. Was möglich:
- Identifikation unklar?
- Daten zu klein?
- Methode wählt sich nicht?
- CATE-Heterogenität nicht interpretierbar?

## Empfohlene Foliengliederung (8–12 Folien)
1. Frage in einem Satz + Motivation.
2. Daten und Setting.
3. DAG + Identifikationsannahmen.
4. Methode(n) + Begründung.
5. Deskriptive Statistik / Overlap.
6. Hauptergebnis (Schätzer + KI).
7. Vergleich Methoden / Spezifikationen.
8. Sensitivity / Robustness (geplant oder erste Resultate).
9. Limitations.
10. Offene Fragen / Bitte um Feedback.
11. (Backup) Technische Details.

## Häufige Fehler in Woche-10-Präsentationen
- **Über-Engineering** der Methode bei vager Frage.
- **Schöne Plots ohne Annahmencheck.**
- **Heterogenitäts-Story ohne Inferenz** auf den Subgruppen.
- **Keine Baseline** — man sieht nicht, was die kausale Methode überhaupt korrigiert.
- **Robustness vergessen** und in Woche 11 hektisch nachschieben.

## Self-Test (für die Generalprobe)
1. Kannst du in 30 Sekunden Frage, Treatment, Outcome, Methode und Hauptergebnis sagen?
2. Wenn ich dir sage: *„Ich glaube, da gibt es einen unbeobachteten Confounder X"* — was antwortest du?
3. Wenn deine Annahmen falsch wären — welche Zahl in deinem Ergebnis ändert sich am stärksten?
4. Was wäre dein **Plan B**, wenn deine Hauptmethode nicht hält?

## Verbindung zu Woche 11
Feedback aus Woche 10 in Robustness, Klarheit und Story einarbeiten. Woche 11 ist **die** Präsentation — bis dahin: Sensitivity-Analysen abschließen, Folien straffen, eine klare Take-home-Message formulieren.
