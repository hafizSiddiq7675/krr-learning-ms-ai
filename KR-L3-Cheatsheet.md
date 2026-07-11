# KR Lecture 3 — Cheat Sheet (Exam-Ready)

> Knowledge Representation & Reasoning — Dr. Husnain Ashfaq
> Covers: Why Rules Fail · Semantic Networks · Hierarchies · Frames · Ontologies · OWL
> Midterm: first 3 lectures

---

## THE BIG ARC (memorize this one line)

> Rule-based AI **breaks** → we keep adding **structure** to fix it.
> **Rules → Semantic Networks → Hierarchies → Frames → Ontologies/OWL.**
> Each step keeps what worked, fixes what broke.

---

# PART 1 — Why Rules Fail

**Rule-based AI** = knowledge stored as `IF (condition) → THEN (action)` (production rules).

- **Forward Chaining** = start from FACTS → apply rules → reach GOAL. *Data-driven.* (business rule engines)
- **Backward Chaining** = start from GOAL → work back to facts that prove it. *Goal-driven.* (Prolog, medical diagnosis)

**MYCIN** = 1970s Stanford rule-based expert system. ~600 production rules, 65–80% accuracy, diagnosed bacterial infections. Success proved rules work; failures motivated everything else.

### The 5 Fundamental Problems ⭐ (mnemonic: **B-N-B-N-C**)
| # | Problem | One line |
|---|---|---|
| 1 | **Bottleneck** (knowledge acquisition) | 600 rules = 3 yrs of interviews; can't scale |
| 2 | **No structural knowledge** | matches patterns, doesn't know what 'bacteria' IS; facts isolated |
| 3 | **Brittleness w/ missing data** | one missing condition → whole chain breaks (all-or-nothing) |
| 4 | **No reuse / no shared vocabulary** | "MI" vs "Heart Attack" can't match across systems |
| 5 | **Combinatorial explosion** | 50 diseases × 30 symptoms = **1,500** min; with combos **2³⁰ ≈ 1 billion** |

⚠ **2³⁰ not 2⁵⁰:** exponent = # of things that COMBINE = **30 symptoms** (each present/absent). Diseases are answer choices, not switches.

Each problem previews a fix: 2→Semantic Nets, 3→Frames (defaults)+Open World, 4→Ontologies, 5→Inheritance.

---

# PART 2 — Semantic Networks

**Definition:** a **directed graph** where **nodes = concepts**, **edges = named relationships**. *The structure IS the knowledge.* Answer queries by **traversal** — no rules.

### Edge types (Slide 14)
| Edge | Meaning | Example |
|---|---|---|
| **is-a** | subclass (class→class) | Dog is-a Mammal |
| **instance-of** | individual→class | [Fido] instance-of Dog |
| **has-a** | part/component | Dog has-a Tail |
| **can** | capability | Bird can Fly |
| **CANNOT** | exception override | Penguin CANNOT Fly |
| lives-in / made-by / compatible-with | habitat / maker / interop | Whale lives-in Ocean |

⚠ **is-a vs instance-of:** is-a links *categories*; instance-of links a *real individual* (in `[brackets]`) to its category. Confusing them = nonsense.

### The 3 reasoning rules ⭐
1. **Inheritance** — properties flow UP/down is-a edges (Fido inherits "breathe" from Animal).
2. **Transitivity** — A→B, B→C ⟹ A→C (is-a chains connect).
3. **Specificity** — a node's OWN/direct edge beats an inherited one (Penguin CANNOT Fly wins over Bird can Fly).

### Traversal method (THE exam skill)
1. Start at queried node. 2. **Check its own direct edges first** (specificity). 3. Else climb is-a/instance-of upward. 4. Stop when found (or run out → answer NO). 5. Count hops.

> "Can Fido breathe?" → [Fido]→Labrador→Dog→Mammal→Animal→can→Breathe = **YES, 4 hops, 0 rules.**
> "Can Penguin fly?" → Penguin→CANNOT→Fly = **NO, 1 hop (specificity).**

### Scale
Google KG (500B facts), LinkedIn (900M people), Amazon (350M products). **Traversal = O(edges); rule-checking = O(rules×facts).** A knowledge graph = a semantic network at industrial scale (+ RDF standard + SPARQL).

**Semantic network vs Knowledge Graph:** same idea; KG = large-scale, formal (RDF), industrial version. Every KG is a semantic net; not every semantic net is a KG.

---

# PART 3 — Concept Hierarchies & Inheritance

**Concept hierarchy** = the pure **is-a backbone** of a semantic net, strict general→specific in levels (L0 Thing … L5 instances). Purpose: enable **inheritance**.

### Inheritance (Slide 26)
Properties defined high flow DOWN to all descendants. [Buddy] inherits has-cells (Animal), warm-blooded (Mammal), barks (Dog)… → **store 1 fact, get 4 free.**

### Specificity / exceptions (Slide 27)
Most specific (closest) edge wins. Store general rule once (Birds fly), add exceptions (Penguin CANNOT) — don't redefine everything.

### Redundancy debugging ⭐ (signature Part-3 question, Slide 30)
Classify each fact: **CORRECT&NEEDED / REDUNDANT / WRONG / MISSING.**
- `Animal.can-breathe=TRUE` → CORRECT (the source)
- `Dog.can-breathe=TRUE` → REDUNDANT (inherited via Mammal→Animal)
- `Penguin.can-fly=TRUE` → WRONG (should be FALSE override)
- `Bat.can-fly` undefined → MISSING (should be TRUE)
- Reduce 9 facts → 6 = 33% saved.

### Design recipe — **H-P-I-O**
**H**ierarchy (is-a, 4–5 levels) · **P**roperties placed *as high as universally true* · **I**nheritance trace to a leaf (count inherited vs stored) · **O**verride (≥1 exception beating an inherited property).
⚠ Top traps: put each property at correct height; inheritance flows **down a branch only, never sideways**.

---

# PART 4 — Frames (Minsky, 1975)

**Frame** = structured template (like an OOP class/object) representing a concept as named **slots**. Analogy: restaurant template (menu, order, pay).

### Slot anatomy (7 parts)
**Data:** slot-name · value · **default** · range-constraint (e.g. year:1900–2030).
**Procedures (procedural attachments):**

| Trigger | Fires when you... | Like | Example |
|---|---|---|---|
| **IF-NEEDED** | **READ** a missing value (computed, never stored) | `@property` / formula | age = today − birth_date |
| **IF-ADDED** | **WRITE/SET** a value | DB trigger / setter | dept set → create Slack + assign mentor |
| **IF-REMOVED** | **DELETE** a value | destructor / cleanup | end-date removed → deactivate badge, archive email |

> Hook: **Read→Needed, Write→Added, Delete→Removed.**

### Frame inheritance (Slide 35)
Inherit via IS-A; children **override** parent slots (specificity). GREEN=inherited, YELLOW=overridden.
Vehicle(num-wheels:4) → Car(4, inherited) · Motorcycle(2, override) · Bicycle(2 + needs-fuel=FALSE, override).

### Why frames matter
**DEFAULTS** fix missing-data brittleness (Problem 3). **RANGE** catches some bad values. **Procedures** add active behavior (graphs were passive).
⚠ Still can't catch **WRONG data** (confident bad value) → motivates ontology constraints.
**MISSING vs WRONG:** missing → default kicks in safely; wrong → used with full confidence, undetected = **more dangerous** (esp. medicine).

---

# PART 5 — Ontologies

**Ontology = VOCABULARY + STRUCTURE + RULES**, in a machine-readable formal language, shared & reasoned over.
**Hook:** Ontology = Semantic Network + Frames + Formal Logic + Standards.

### 4 properties
**FORMAL** (precise lang OWL/RDF) · **EXPLICIT** (nothing implied) · **SHARED** (multi-org single truth) · **CONCEPTUALIZATION** (models domain, an abstraction).

### RDF — the data layer
**RDF triple** = one fact = **Subject → Predicate → Object** = one edge.
`<Babar> <nationality> <Pakistan>` · `<Alice> <worksAt> <Google>`.
Object can be a **resource** (a thing → grows graph) or a **literal** (raw value, e.g. `hasAge 28` = data property). Query with **SPARQL**.

### OWL — the logic layer (adds rules a reasoner enforces)
subClassOf, disjointWith, cardinality, FunctionalProperty, auto-inference (Doctor⊑Employee ⟹ Alice the Doctor is an Employee, never stated).

### Open World vs Closed World ⭐⭐
| | Closed (DB / SQL / Prolog) | Open (OWL) |
|---|---|---|
| Unstated fact | assumed **FALSE** | **UNKNOWN** (not false) |
| No allergy recorded | "safe to prescribe" ⚠️ | "check before prescribing" ✅ |
| Use | complete data | real-world incomplete data |
> **Absence of data ≠ absence of fact.** Open World = safe assumption.

---

# PART 6 — OWL Modeling Concepts (Capstone)

### subClassOf vs rdf:type ⭐⭐ (#1 trap)
| `rdf:type` (instance) | `subClassOf` (class) |
|---|---|
| `:Fido rdf:type :Dog` (Fido is an individual Dog) | `:Dog subClassOf :Mammal` (Dog ⊆ Mammal) |
| Python: `fido = Dog()` | Python: `class Dog(Mammal)` |
⚠ Writing `:Fido subClassOf :Dog` makes Fido a **class** → then `:Rex rdf:type :Fido` = nonsense.

### 8 OWL constructs (Slide 54) ⭐
| Construct | Meaning | Example |
|---|---|---|
| equivalentClass | classes = same set | HeartAttack ≡ MI |
| sameAs | individuals = same | MuhammadAli sameAs CassiusClay |
| disjointWith | share NO members | Man ⊥ Woman |
| TransitiveProperty | A→B,B→C ⟹ A→C | isPartOf, isLocatedIn |
| FunctionalProperty | at most 1 value | hasBiologicalMother |
| SymmetricProperty | A→B ⟹ B→A | isSiblingOf, isMarriedTo |
| minCardinality n | at least n | Course minCard 1 student |
| cardinality n | exactly n | Triangle cardinality 3 sides |

### Reasoner (HermiT / Pellet) — 4 jobs ⭐
1. **Classification** — properties → place class in hierarchy (Platypus → Mammal).
2. **Consistency check** — find contradictions (Alice as Man+Woman, disjoint → INCONSISTENT).
3. **Instance classification** — Bob hasChild + Male → infers Father.
4. **Property inference** — transitive/symmetric/functional (Valve isPartOf Heart isPartOf Body ⟹ Valve isPartOf Body).
> Reasoner **derives facts you never stated.**

### Consistency debugging ⭐⭐ (signature Part-6 Q) — 3 contradiction sources
- **disjointWith** violation (typed as both disjoint classes)
- **cardinality 1** violated (2 values exist) → fix: change to **minCardinality 1**
- bad **subClassOf** chain (FullProfessor⊑Man + a female FullProf) → fix: delete the bad axiom
- VitaminC as both Drug & Food (disjoint) → fix: `NutraceuticalDrug subClassOf Drug`.

### History (Slide 61)
1950s–70s Rules/MYCIN → 70s–80s Semantic Nets+Frames (Quillian, Minsky) → 90s Description Logics → 2000s RDF+OWL (W3C, Semantic Web) → 2010s Industrial KGs (Google/LinkedIn/Amazon) → 2020s LLMs+KGs (Neuro-Symbolic AI).

---

## ⭐ MOST-LIKELY EXAM QUESTIONS (priority order)
1. **Traversal queries** — given a network, answer 6–8 with full paths + hops (Activity 2.2).
2. **Open World vs Closed World** + medical example.
3. **subClassOf vs rdf:type** — why `Fido subClassOf Dog` is wrong.
4. **Consistency check** — find + fix contradictions (disjoint/cardinality/subclass).
5. **"What will the reasoner infer?"** — transitive/subclass chain.
6. **The 5 problems** of rule-based AI (B-N-B-N-C) + which caused a given failure.
7. **Design a semantic net / hierarchy / ontology** (R-C-I-P-E-L / H-P-I-O + OWL axioms).
8. **Frame reasoning** — apply defaults, fire IF-NEEDED, compute (triage).

## QUICK MNEMONICS
- 5 problems = **B-N-B-N-C** (Bottleneck, No-structure, Brittleness, No-reuse, Combinatorial).
- 3 graph rules = **Inheritance, Transitivity, Specificity.**
- Frame procedures = **Read→Needed, Write→Added, Delete→Removed.**
- Ontology 4 = **FESC** (Formal, Explicit, Shared, Conceptualization).
- Design recipes = **R-C-I-P-E-L** (graph) / **H-P-I-O** (hierarchy).
- RDF = **S-P-O** (Subject-Predicate-Object).
