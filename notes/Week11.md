# Woche 11 — Final Presentations

## Material im Repo
- **README:** leer.
- Keine Vorlesung — diese Woche ist die **Abschlusspräsentation** des eigenen Projekts.

## Was die Woche leisten soll
Eine klare, ehrliche, methodisch korrekte 10–15-Minuten-Präsentation deiner Projektarbeit. Bewertet wird in solchen Kursen typischerweise:
- **Frage & Setting** (Klarheit, Relevanz),
- **Identifikation** (Annahmen explizit, plausibel begründet),
- **Methode** (passt zu Frage und Daten, korrekt umgesetzt),
- **Ergebnisse** (mit Unsicherheit, mit Vergleichen),
- **Robustness / Sensitivity**,
- **Reflexion** (Limitations, offene Fragen).

## Storyline (eine Erzählung, keine Methodensammlung)

Eine gute Präsentation liest sich wie folgt:

> *„Mich interessiert **Frage F**. Das ist relevant, weil **Begründung B**. Ich nutze **Daten D**, weil sie **Eigenschaft E** haben. Der naive Schätzer würde **Bias X** liefern, weil **Confounder/Selection-Mechanismus Y**. Mit **Methode M** unter **Annahmen A** identifiziere ich den Effekt. Mein Ergebnis ist **$\hat\theta = …$ (95 %-KI: …)**. Robustness gegen **R1, R2, R3** zeigt: **Ergebnis hält** / **Ergebnis bricht bei …**. Take-home: **eine konkrete Aussage**."*

Wenn dieser Absatz nicht in 60 Sekunden flüssig kommt, ist die Präsentation noch nicht fertig.

## Foliengliederung (12–15 Folien)

1. **Titel** + 1-Satz-Frage.
2. **Motivation** — warum interessiert das jemanden außerhalb des Kurses?
3. **Setting & Daten.**
4. **DAG / Identifikationsstrategie.**
5. **Methode** — knapp, mit Begründung.
6. **Deskriptiv / Overlap / Pre-Trends.**
7. **Hauptergebnis** — eine Zahl, ein Plot, ein KI.
8. **Heterogenität (CATE)** — falls relevant.
9. **Robustness 1** (alternative Spezifikation).
10. **Robustness 2** (Sensitivity / Placebo).
11. **Limitations** — ehrlich, nicht defensiv.
12. **Take-home** — was die Welt mitnehmen sollte.
13. (Backup) Daten-/Code-/Methoden-Details.

## Präsentations-Tipps

- **Einsatz der Zeit**: ⅓ Frage & Setup, ⅓ Methode & Hauptergebnis, ⅓ Robustness & Diskussion.
- **Plots > Tabellen**. Eine Tabelle pro Talk reicht.
- **Konfidenzintervalle visuell** (Errorbars), nicht in Sternchen.
- **Eine Botschaft pro Folie.** Wenn zwei Punkte drauf sind, splitte.
- **Methoden-Slide** zeigt Architektur, nicht Code.
- **Q&A-Vorbereitung:** drei wahrscheinliche kritische Fragen + überzeugte, aber ehrliche Antworten.

## Likely Questions (schon mal vorab beantworten)

1. **„Sind deine Annahmen plausibel?"** — Welche genau, warum, und wie hast du das geprüft?
2. **„Was, wenn dein Hauptconfounder unbeobachtet ist?"** — Sensitivity-Zahl nennen (E-Value, Cinelli-Hazlett-Bound).
3. **„Warum diese Methode und nicht jene?"** — kurze Begründung mit Bezug auf Datenstruktur und Annahmen.
4. **„Hat dein Ergebnis externe Validität?"** — Setting-Spezifik vs. Generalisierbarkeit.
5. **„Gibt es Spillover/SUTVA-Bedenken?"** — Falls ja, wie behandelt; falls nein, warum nicht.

## Selbst-Review vor der Abgabe

- [ ] Frage in einem Satz formuliert?
- [ ] Identifikationsannahmen explizit auf einer Folie?
- [ ] DAG vorhanden und konsistent mit Methode?
- [ ] Mindestens eine Baseline (naive Methode) zum Vergleich?
- [ ] Konfidenzintervalle überall, wo Schätzer auftauchen?
- [ ] Mindestens zwei Robustness-Checks?
- [ ] Sensitivity gegen unbeobachtete Confounder gequantifiziert?
- [ ] Limitations-Folie mit echten Schwachstellen, nicht Floskeln?
- [ ] Take-home in einem Satz?
- [ ] Alle Plots beschriftet, lesbar, ohne Default-Matplotlib-Titel?
- [ ] Code reproduzierbar (Seed, Daten-Quelle, Versionen)?

## Nach der Präsentation

- Feedback notieren, **nicht** verteidigen.
- Falls Energie-Daten verwendet: überlegen, ob sich daraus eine Veröffentlichung / Blog-Post / interner Bericht ableitet.
- Notes (Woche 1–11) als persönliches Cheat-Sheet behalten — sie sind die Grundlage für jeden zukünftigen kausalen Use Case in deiner Arbeit.

## Lernziel-Check des gesamten Kurses

Nach Woche 11 solltest du in der Lage sein:
1. Eine kausale Frage von einer prädiktiven Frage zu unterscheiden.
2. Aus einem realen Setting ein DAG abzuleiten und ein valides Adjustment-Set zu identifizieren.
3. Zwischen ATE-, ATT-, CATE-Schätzern bewusst zu wählen.
4. Double ML / Doubly Robust / Causal Forest praktisch zu implementieren.
5. Die Standard-Identifikationsannahmen zu nennen und Verletzungen zu erkennen.
6. Sensitivity-Analysen einzubauen.
7. Granger ≠ Pearl flüssig zu erklären.
8. Ein eigenes kausales ML-Projekt von Frage über Methode bis Robustness durchzuziehen.

Wenn du auf alle acht Punkte „ja" sagen kannst — Kurs erfolgreich.
