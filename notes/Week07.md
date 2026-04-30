# Woche 7 — Graph-Kausalität (DAGs, do-Kalkül, Causal Discovery)

## Material im Repo
- **Artikel:**
  - Towards Data Science: *Causal Discovery* — https://towardsdatascience.com/causal-discovery-6858f9af6dcb/
  - Medium / TDS: *Using Causal Graphs to Answer Causal Questions*
  - Frontiers in Genetics: methodischer Überblick zu Causal Inference in Genetik (gute Discovery-Beispiele).
- **Videos:** vier Videos zu DAGs / do-Kalkül / Discovery (Pearl-Linie).
- **Notebooks (CausalAIBook):**
  1. `python-colliderbias-hollywood.ipynb` (CM2) — Collider Bias didaktisch.
  2. `python-pgmpy.ipynb` (CM3) — DAG-Modellierung mit `pgmpy`.
  3. `python-dosearch.ipynb` (CM3) — Identifikation per do-Kalkül-Algorithmus.

## Was die Woche leisten soll
Die **Graph-Sprache** der Kausalität fließend lernen: DAGs lesen, schreiben, daraus Adjustment-Sets ableiten, Identifikation prüfen — und einen ersten Kontakt mit **Causal Discovery** (Struktur lernen statt vorgeben).

## Kernkonzepte

### 1. DAG-Grundlagen
Ein **directed acyclic graph** kodiert Annahmen über kausale Mechanismen. Drei elementare Strukturen:
- **Chain** $X \to M \to Y$: $M$ ist Mediator. Bedingen auf $M$ blockt den indirekten Pfad.
- **Fork** $X \leftarrow C \to Y$: $C$ ist Confounder. Bedingen auf $C$ blockt den Backdoor.
- **Collider** $X \to K \leftarrow Y$: $K$ ist Collider. Bedingen auf $K$ **öffnet** einen Pfad — gefährlich.

### 2. d-Separation
Ein Pfad ist **blockiert** durch eine Konditionierungsmenge $Z$, wenn:
- ein Nicht-Collider auf dem Pfad in $Z$ liegt, **oder**
- ein Collider auf dem Pfad **nicht** in $Z$ und auch keiner seiner Nachfolger in $Z$ liegt.

$X \perp Y \mid Z$ im Graph ⇔ $X \perp Y \mid Z$ in der Verteilung (Markov + Faithfulness).

### 3. Backdoor-Kriterium (Pearl)
$Z$ ist eine **valide Adjustment-Menge** für den Effekt $T \to Y$, wenn:
1. $Z$ enthält keinen Nachkommen von $T$,
2. $Z$ blockiert alle Backdoor-Pfade von $T$ nach $Y$.

Dann ist:
$$
P(Y \mid do(T=t)) = \sum_z P(Y \mid T=t, Z=z)\,P(Z=z).
$$

### 4. do-Kalkül (drei Regeln)
Pearls drei Regeln zur Manipulation von Ausdrücken mit $do$-Operator. Hauptanwendung: **Identifikation** — kann ein kausaler Ausdruck rein aus Beobachtungsdaten geschätzt werden? Algorithmen: **ID-Algorithm**, implementiert u.a. in `dosearch` (R) und in Causal-Fusion / `Y0`.

### 5. Front-Door-Kriterium
Wenn Backdoor-Adjustment unmöglich ist (unbeobachteter Confounder), kann der Effekt manchmal trotzdem identifiziert werden — über einen Mediator $M$, der vollständig zwischen $T$ und $Y$ steht und selbst nicht confoundet ist:
$$
P(Y\mid do(T)) = \sum_m P(M\mid T)\sum_{t'} P(Y\mid M=m, T=t')P(T=t').
$$

### 6. Collider Bias (Notebook „Hollywood")
Klassisches Beispiel: in Hollywood sind Looks und Talent negativ korreliert — nicht in der Realität, sondern weil **Erfolg = Looks ∨ Talent** und wir nur Erfolgreiche beobachten (Selection auf Collider). Selection ist ein häufiger versteckter Collider-Bias-Mechanismus.

### 7. Causal Discovery (Strukturlernen)
Wenn wir den Graph nicht kennen — kann man ihn aus Daten lernen?
- **Constraint-based:** PC-Algorithmus, FCI (nutzen Conditional Independence Tests).
- **Score-based:** GES, NOTEARS (kontinuierliche Optimierung der Adjazenzmatrix).
- **Functional Causal Models:** LiNGAM (lineare Modelle, nicht-Gaußsches Rauschen), ANM, CAM.

Praktisch: Discovery liefert oft **Markov-Äquivalenz-Klassen** (mehrere DAGs sind gleich konsistent mit den Daten). Ohne Annahmen kommt man nicht weit; mit Annahmen (Linearität, Nicht-Gaußheit, additives Rauschen, Interventionen) wird es identifizierbar.

## Fallstricke
- **Bedingen auf einen Collider** (= „Bad Control") — verzerrt eher, als dass es hilft. Häufig in Studien zu Selektionsmechanismen.
- **DAG-Annahmen nicht expliziten machen** — der Effekt-Schätzer ist immer **bedingt auf das angenommene DAG**.
- **Faithfulness-Verletzung:** im Graph implizierte Abhängigkeit, die in den Daten zufällig kanzelliert. Discovery scheitert dann.
- **Discovery-Output als Wahrheit lesen:** typischerweise ist das Ergebnis eine Äquivalenz-Klasse, kein eindeutiges DAG.

## Self-Test
1. Zeichne ein DAG mit einem Confounder, einem Mediator und einem Collider zwischen $T$ und $Y$. Welche Variablen sind valides Adjustment, welche nicht?
2. Was ist der Unterschied zwischen $P(Y \mid T=t)$ und $P(Y \mid do(T=t))$ — und wann sind sie gleich?
3. Erkläre Front-Door-Adjustment an einem Beispiel (Klassiker: Rauchen → Teer → Lungenkrebs).
4. Warum liefert PC-Algorithmus eine **CPDAG**, kein DAG?

## Verbindung zu Woche 8
Woche 8 bringt die Graph-Sprache in die Welt vernetzter Einheiten — **Network Causality**: Was passiert, wenn die SUTVA-Annahme verletzt ist, weil meine Behandlung deine Outcomes beeinflusst (Spillover, Peer-Effekte)?
