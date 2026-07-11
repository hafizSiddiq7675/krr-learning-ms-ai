# KR Lecture 4 — Cheat Sheet (Exam-Ready)

> Knowledge Representation & Reasoning — Dr. Husnain Ashfaq
> Covers: Semantic Web · RDF & OWL · SPARQL · Probability & Bayes · Markov & Fuzzy
> Final term: Lectures 4 + 5

---

## THE BIG ARC (memorize this one line)

> Lecture 4 = **two halves.**
> **Half 1 (Parts 1–3): store & query CERTAIN knowledge** — Semantic Web → RDF triples → OWL rules → SPARQL queries.
> **Half 2 (Parts 4–5): reason under UNCERTAINTY** — Probability → Bayes → Bayesian Nets → Markov → Fuzzy.
> Quick selector: **storing facts→RDF · rules→OWL · querying→SPARQL · updating beliefs→Bayes · cause+effect→BayesNet · sequences→Markov · vague words→Fuzzy.**

---

# PART 1 — The Semantic Web

**Problem with old web:** search = **word matching**, not meaning. "Father of Albert Einstein" → finds pages containing 'father' + 'Einstein'. Computer sees WORDS not MEANING.
**Semantic Web:** facts stored with meaning → `Hermann Einstein —FATHER OF→ Albert` → direct answer.

### Three Generations of the Web ⭐ (classification is a signature question)
| Gen | Nickname | Traits | Examples |
|---|---|---|---|
| **Web 1.0** (1990s) | The Library | read-only, static HTML | 1998 uni homepage, 2001 product catalogue |
| **Web 2.0** (2004+) | Library + Café | read AND write, social | Facebook, YouTube, Wikipedia, WhatsApp, Reddit voting |
| **Web 3.0** (today) | Smart Library | machines understand meaning, direct answers | Google KG answer box, Siri, Wikidata RDF triples |

⚠ **Trap:** Wikipedia/WhatsApp feel "smart" but are **2.0** — interactive, yes; machine-understood meaning, no. 3.0 needs **semantic markup (RDF)** so machines grasp entities + relationships.

### Layer Cake — 7 layers, build BOTTOM-UP
1. **XML/Unicode** (alphabet & grammar) → 2. **URI** (unique web ID for every thing) → 3. **RDF** (facts as triples) → 4. **RDFS** (basic types/schemas) → 5. **OWL** (what categories MEAN) → 6. **Rules/SWRL** (IF-THEN) → 7. **Trust** (can I believe it?)
> Hook: **eXtra URIs Really Require OWLs, Rules & Trust.**

**Google Knowledge Graph** = Semantic Web in action: **500 billion facts**, linked data, direct answers (Eiffel Tower card: location, height 330m, architect…) — no website click needed.

---

# PART 2 — RDF & OWL

### RDF — every fact = one TRIPLE ⭐
**Subject → Predicate → Object** (like grammar: "Ali studies ComputerScience").
`Lahore isA City` · `Lahore locatedIn Pakistan` · `Pakistan capital Islamabad`.
Draw as graph: **nodes = things, arrows = relationships**.
**URI** = unique ID for every thing (**like a CNIC for web resources**) — `<http://dbpedia.org/resource/Lahore>` is globally unambiguous; the word 'Lahore' alone is not.

### Turtle syntax — 4 punctuation rules ⭐
| Symbol | Means | Example |
|---|---|---|
| `.` period | END of one complete triple | `Lahore isA City .` |
| `;` semicolon | same SUBJECT, new predicate | `Lahore isA City ; population 14M .` |
| `,` comma | same subject AND predicate, new OBJECT | `Lahore famousFor Food , History .` |
| `@prefix` | shortcut for long URI | `@prefix ex: <http://example.org/> .` |

⚠ **Comma vs semicolon:** comma = only the object changes; semicolon = predicate changes too.

### RDF stores FACTS, OWL stores the RULES about facts (the instruction manual)
**Ontology** = formal description of a domain: what THINGS exist, what TYPES, what RELATIONSHIPS. Dictionary + rulebook.
Real example: **SNOMED CT** medical ontology, **350,000+ concepts**, used by NHS/Australia. Drug treats 'ViralInfection' → OWL auto-knows it may help COVID-19 (subclass) — nobody adds that link manually.

### OWL class constructs
| Construct | Meaning | Example |
|---|---|---|
| **subClassOf** | hierarchy + inheritance | Dog subClassOf Mammal |
| **disjointWith** | nothing can be BOTH | Dog ⊥ Cat |
| **equivalentClass** | two names, same set | Car ≡ Automobile |
| **unionOf** | member of at least one | Parent = Father OR Mother |

### OWL properties — 2 kinds + 3 behaviours ⭐
- **ObjectProperty** = THING→THING (`Ali worksAt FAST`) · **DatatypeProperty** = THING→VALUE (`Ali age 25`).
- **Transitive** (chains: Ali ancestor Bilal, Bilal ancestor Carla ⟹ Ali ancestor Carla) · **Symmetric** (works both ways: isSiblingOf, isMarriedTo) · **Functional** (ONE value only: hasBiologicalMother — 2nd value = error).

### The Reasoner ⭐ (signature question: "what will it infer?")
- **Entailment up the chain:** `Ahmed rdf:type Surgeon` + Surgeon⊑Doctor⊑HealthcareWorker ⟹ Ahmed is a Doctor AND a HealthcareWorker — automatically.
- **Inconsistency detection:** Tux is NonFlyingBird; add `Tux rdf:type Eagle` (a FlyingBird, disjoint) → **INCONSISTENCY flagged.** This is a FEATURE — catches data-entry errors (hospitals use it on patient records).
- Zoo chain: Simba→Lion→Mammal→Animal→LivingThing = **3 inferences from 1 stated fact.**

---

# PART 3 — SPARQL

**Pattern matching on triples.** `?x` = **variable** (a blank to fill). SPARQL finds ALL combinations matching the pattern simultaneously.

### 4 query types ⭐ (mnemonic: **S-A-C-D**)
| Type | Returns | Use when |
|---|---|---|
| **SELECT** | a TABLE of results | "find all…" |
| **ASK** | TRUE/FALSE only | "does at least one exist?" — need existence, not data |
| **CONSTRUCT** | a NEW set of triples | build fullName from first+last |
| **DESCRIBE** | ALL triples about one resource | "everything about :Islamabad" |

⚠ **ASK vs SELECT trap:** "Has DrKhan ever taught AI101?" → ASK. SELECT is WRONG because you don't need a table, just yes/no. Pattern: `ASK { :DrKhan :taught :AI101 . }`

### Keywords ⭐
- **FILTER** = sieve, numeric/logic condition: `FILTER(?rating > 8.6)` · `FILTER(?cgpa > 3.5)`.
- **OPTIONAL** = **like SQL LEFT JOIN**: keep rows even when data missing (students without phone still appear, cell empty). Without it, missing value = whole row vanishes. ⚠ Hospital blood-type question = OPTIONAL.
- **COUNT / GROUP BY / ORDER BY**: `SELECT ?dir (COUNT(?movie) AS ?n) WHERE {…} GROUP BY ?dir ORDER BY DESC(?n)`.
- Multiple triple patterns in WHERE are **AND-ed** — all must match: `{ ?s isA :Student . ?s enrolled :CS . ?s cgpa ?g . FILTER(?g > 3.5) }`

---

# PART 4 — Probability & Bayes' Theorem

### Why classical logic fails (5 problems)
Light switch (ON/OFF) vs real world needing a **DIMMER (0→1)**:
**M-U-C-V-W:** **M**issing info (car not in DB ≠ no car) · **U**ncertain data (thermometer ±0.5°) · **C**onflicting rules (penguin/bird) · **V**ague words (is Lahore 'big'?) · **W**orld changes (facts expire).
Fix = **Probability** (0–1) + **Fuzzy Logic**.

### 3 probability rules
1. **Complement:** P(NOT A) = 1 − P(A). All probabilities sum to 1.
2. **AND (independent):** P(A AND B) = P(A) × P(B). Both must happen → multiply.
3. **Conditional:** P(A|B) = prob of A GIVEN B happened. **The `|` shrinks the universe** — P(Ace | Spade) = 1/13 (universe = 13 spades).

### Bayes' Theorem ⭐⭐⭐ (THE formula)
> **P(H|E) = P(E|H) × P(H) / P(E)**

| Part | Name | Meaning |
|---|---|---|
| P(H) | **Prior** | belief BEFORE evidence |
| P(E\|H) | **Likelihood** | how likely evidence if H true |
| P(E) | **Evidence prob** | overall chance of E from ALL causes = P(E\|H)P(H) + P(E\|¬H)P(¬H) |
| P(H\|E) | **Posterior ★** | updated belief = THE ANSWER; becomes new prior for next evidence |

### The 3-step method ⭐⭐⭐ (THE exam skill — always show all 3)
1. **List** known values (incl. P(¬H) = 1 − P(H)).
2. **Total probability:** P(E) = P(E|H)×P(H) + P(E|¬H)×P(¬H).
3. **Apply formula** + interpret.

**Worked 1 — Disease:** P(D)=0.01, P(+|D)=0.95, P(+|¬D)=0.10.
P(+) = 0.95×0.01 + 0.10×0.99 = 0.0095 + 0.099 = **0.1085**. P(D|+) = 0.0095/0.1085 ≈ **8.76%**.
**Worked 2 — COVID:** P(C)=0.05, P(+|C)=0.90, P(+|¬C)=0.08.
P(+) = 0.045 + 0.076 = **0.121**. P(C|+) = 0.045/0.121 ≈ **37.2%** → get a second test (posterior becomes new prior → ~92%+ after 2 positives).
**Worked 3 — Spam:** P(S)=0.4, P(FREE|S)=0.80, P(FREE|¬S)=0.05.
P(FREE) = 0.32 + 0.03 = **0.35**. P(S|FREE) = 0.32/0.35 ≈ **91.4%**. Gmail chains hundreds of words (posterior→new prior each time) → 99.9%.

### Base Rate Effect ⭐⭐ (explain-in-English question)
Test 95% accurate ≠ 95% sick. Disease is RARE → the huge healthy group × small false-alarm rate produces **far more false positives than true positives**. 1 crore people: 95,000 true positives vs 9,90,000 false ones → 95,000/10,85,000 = 8.76%. **The prior (rarity) dominates.** COVID gave 37.2% not 8.76% because 5% base rate > 1% — less rare = positives more meaningful.

### Bayesian Networks — burglar alarm ⭐
Graph of causes→effects with probabilities. Burglary(0.001) + Earthquake(0.002) → **Alarm** → JohnCalls, MaryCalls.
**CPT** (conditional probability table) = given by domain experts, not calculated. B,E→0.95 · B only→0.94 · E only→0.29 · neither→0.001.
- **Forward (predictive) inference:** cause→effect. P(John calls | Burglary) = 0.94 × 0.90 ≈ **85%**.
- **Backward (diagnostic) inference:** effect→cause. John+Mary call → P(Burglary): 0.001 → 1.67% (John) → **28.4%** (Mary too; each evidence updates, posterior becomes prior).
⚠ "John calls, what about burglary?" = **BACKWARD** — observing an effect, reasoning to cause; P(Burglary) INCREASES.

---

# PART 5 — Markov Models & Fuzzy Logic

### Markov Property ⭐
> Next state depends **ONLY on the CURRENT state** — history is irrelevant.
Board game: token on square 34 — took 5 or 50 moves to get there, doesn't matter. Chess: only the current board position matters; history is already embedded in the present.

### Transition matrix
Row = FROM, column = TO. ⚠ **Every ROW sums to 1.0** (must go somewhere).

### Multi-step calculation ⭐⭐⭐ (THE exam skill)
> **MULTIPLY within a path, ADD across paths.**
Rainy today → P(Sunny in 2 days)? 3 paths (day-1 can be S/R/C):
R→S→S: 0.30×0.70 = 0.21 · R→R→S: 0.40×0.30 = 0.12 · R→C→S: 0.30×0.50 = 0.15 → total **0.48**.
Why multiply within: both steps must happen in sequence. Why add across: any route counts.
E-commerce: P(Cart→Checkout→Buy) = 0.5 × 0.7 = **35%** (exactly-2-steps = single path = just multiply).
Customer journey insight: Browse has highest Leave rate (50%) → fix UX there.

### Hidden Markov Models (HMM) ⭐
Markov chain = states VISIBLE. HMM = states **HIDDEN**; you only see **observations** the state CAUSES.
| Probability type | What it does | Example |
|---|---|---|
| **Transition** | hidden state → next hidden state | Healthy→Sick 0.30 (rows sum to 1) |
| **Emission** | hidden state → observation | Healthy emits Normal 80% / Tired 20% |

HMM question: given observation sequence (Tired, Tired, Normal, Tired) → most likely hidden sequence (Sick, Sick, Healthy, Sick).
**Speech recognition:** hidden = phonemes, observations = audio frequencies.
⚠ **Spot-the-model trap:** psychologist can't SEE 'Depressed/Stable', only observes 'Engaged/Withdrawn/Tearful' → **HMM** (state unobservable). If you can see the states directly → plain Markov chain.

### Fuzzy Logic ⭐⭐
Classical = light switch; fuzzy = **dimmer** — membership degree μ ∈ [0,1].
Cutoff problem: 180cm rule → Ali 179.9 NOT TALL, Bilal 180.1 TALL — 0.2cm decides?! Fuzzy: μ(tall)=0.85, smooth gradient.
| Operation | Formula | Why |
|---|---|---|
| **AND** | **MIN**(μ₁, μ₂) | both needed → weakest link |
| **OR** | **MAX**(μ₁, μ₂) | at least one → strongest saves you |
| **NOT** | 1 − μ | must sum to 1 |

Worked: μ(tall)=0.7, μ(fast)=0.4 → AND=0.4, OR=0.7, NOT tall=0.3.
Patient: μ(young)=0.6, μ(healthy)=0.8 → AND=0.6 (limited by youth), OR=0.8, NOT young=0.4.

### Fuzzy Inference System — 4 steps ⭐⭐ (mnemonic: **F-R-A-D**)
1. **Fuzzify** — crisp input → membership degrees. 30°C: COOL=0.0, WARM=0.5, HOT=0.3.
2. **Rule evaluation** — each IF-THEN rule fires at its membership strength. WARM→MEDIUM fires 0.5; HOT→FAST fires 0.3; COOL→SLOW silent.
3. **Aggregate** — clip each output shape at its firing strength, combine into one fuzzy region.
4. **Defuzzify** — **centroid** (centre of gravity) of combined shape → one crisp number: **fan = 68%**.

At 22°C (COOL=0.8, WARM=0.2): SLOW dominates → fan ~20–30%.
Cruise control: error +5 → HOLD 0.5 + DECREASE 0.5 → slight decrease; error −8 → INCREASE 0.8 dominates → strong push. **Further from target = stronger correction** — like a human driver.
**Why ABS uses fuzzy:** binary 'IF locked THEN release' = jerky; graduated membership → smooth proportional pressure 14×/sec → shorter stops + steering control.
Real world: rice cookers, autofocus, ABS, AC, washing machines. Invented by **Lotfi Zadeh (1965)**.

---

## ⭐ MOST-LIKELY EXAM QUESTIONS (priority order)
1. **Full 3-step Bayes calculation** (COVID/spam variant) + interpret + base-rate explanation.
2. **Markov 2-step path** — multiply within, add across; show the multiplication.
3. **Fuzzy ops** (MIN/MAX/1−μ) + which rules fire at what strength + rough defuzzified output.
4. **SPARQL**: pick correct query type (ASK vs SELECT trap) + write WHERE pattern + FILTER/OPTIONAL.
5. **Write RDF triples** from English sentences (FAST University style) + draw the graph.
6. **OWL reasoner**: what's inferred (subclass chain / transitive / symmetric) or what contradiction is flagged (disjoint).
7. **Classify Web 1.0 / 2.0 / 3.0** with one reason each.
8. **Forward vs backward inference** in the burglar-alarm network.
9. **Markov vs HMM** — identify hidden states vs observations.
10. **Turtle punctuation** — period/semicolon/comma/@prefix.

## QUICK MNEMONICS
- Layer cake bottom-up = **X-U-R-R-O-R-T** (XML, URI, RDF, RDFS, OWL, Rules, Trust).
- SPARQL types = **S-A-C-D** (SELECT table, ASK T/F, CONSTRUCT triples, DESCRIBE all).
- Classical logic's 5 failures = **M-U-C-V-W** (Missing, Uncertain, Conflicting, Vague, World-changes).
- Bayes parts = **Prior × Likelihood / Evidence = Posterior**; posterior → new prior.
- Markov math = **multiply WITHIN a path, add ACROSS paths**; rows sum to 1.
- Fuzzy = **AND-MIN, OR-MAX, NOT-1minus**; inference = **F-R-A-D** (Fuzzify, Rules, Aggregate, Defuzzify).
- Selector: **vague→Fuzzy · sequence→Markov · cause/effect→BayesNet · update belief→Bayes · rules→OWL · query→SPARQL · store→RDF.**
