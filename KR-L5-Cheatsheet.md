# KR Lecture 5 — Cheat Sheet (Exam-Ready)

> Knowledge Representation & Reasoning — Dr. Husnain Ashfaq
> Covers: Non-Monotonic Reasoning · Commonsense Reasoning · Temporal Reasoning · Knowledge Graphs · Neuro-Symbolic AI
> Final term: Lectures 4 + 5

---

## THE BIG ARC (memorize this one line)

> Lecture 5 = **making AI reason like humans**, in 5 steps:
> **take beliefs back** (Non-Monotonic) → **know the obvious** (Commonsense) → **track time** (Temporal) → **connect facts** (Knowledge Graphs) → **learn AND reason** (Neuro-Symbolic).
> Topics 1–3 = human-style reasoning · Topic 4 = the structure that stores it · Topic 5 = gluing it to neural networks.

---

# TOPIC 1 — Non-Monotonic Reasoning

**Monotonic (classical) logic:** conclusions only ever GROW — `Conclusions(K1) ⊆ Conclusions(K2)`. Adding facts can never invalidate old conclusions (2+2=4 forever).
**Non-monotonic reasoning:** adding new information can cause previously derived conclusions to be **RETRACTED**. Models human reasoning under incomplete info — jump to reasonable conclusions, revise later.

> Hook: **GPS changes its mind.** "Take M-2" → accident reported → reroute. The GPS wasn't wrong; it made the best call with the info available, then **revised**.

### Tweety / Polly ⭐ (signature walkthrough — know the steps)
1. Fact: Tweety is a bird → default rule fires → tentative belief: **Tweety can fly**.
2. New fact: Tweety is a penguin (or Polly has a broken wing) → conflicts with default.
3. Default's "unless" condition now satisfied → rule no longer applies → **belief withdrawn**.
4. Revised: cannot fly. **No contradiction** — the tentative belief was retracted, not fought.
⚠ Classical logic here ends up holding BOTH Fly(Tweety) and ¬Fly(Tweety) — it has no clean take-back mechanism. That's exactly why non-monotonic reasoning exists.

### Default Reasoning (Raymond Reiter)
> **IF condition true AND no evidence against it → assume the conclusion.** ("unless stated otherwise")
`Bird(X) ⇒ Fly(X)` unless contradicted. Without defaults, AI would need COMPLETE knowledge before acting — a robot refusing to move because "I don't know everything about this room."
**Specificity Preference Principle:** the more specific rule ("penguins don't fly") ALWAYS beats the general default ("birds fly").

### CWA vs OWA ⭐⭐ (also in L3/L4 — examiner's favourite bridge)
| Feature | **Closed-World (CWA)** | **Open-World (OWA)** |
|---|---|---|
| Missing fact treated as | **FALSE** | **UNKNOWN** |
| Typical use | Databases (library, banking, student records) | Knowledge Graphs, Semantic Web |
| Assumes KB is | Complete | Possibly incomplete |
| Risk if wrong | False negatives (wrongly says "no") | Less decisive (can't confirm/deny) |

Library example: no record of Ahmed borrowing → CWA says **false**, not unknown. Works ONLY because the DB is genuinely a complete record — CWA **fails badly on open domains** like the web. ⚠ CWA is itself non-monotonic: add Ahmed's loan tomorrow → yesterday's "false" is retracted.

### Belief Revision
The umbrella cycle: **Knowledge → Inference → Conclusion → New Evidence → Belief Revision → Updated Conclusion.** (Sunny→picnic; storm warning→plan changes.) Default reasoning, CWA and TMS all serve this.

### Truth Maintenance System (TMS) ⭐
Tracks **WHY** each belief holds: **Beliefs** (what's true) · **Justifications** (reasons supporting each) · **Dependencies** (links, so removal cascades).
> **Building analogy:** beliefs = floors, justifications = pillars. Remove a pillar → the floor collapses.
Walkthrough: "Road A is best" justified by {no traffic, shortest}. Traffic reported → justification dead → TMS **auto-retracts** "Road A is best" (no full KB re-scan) → recommends Road B.

### Nixon Diamond ⭐⭐ (conflicting defaults)
Rule A: Quakers → pacifist. Rule B: Republicans → NOT pacifist. Richard is BOTH.
Check specificity: neither category is a sub-case of the other → **genuine unresolved conflict** → correct answer: **UNDETERMINED — flag the conflict, don't guess.** A system that picks randomly is dangerous (medical/legal AI).

### Applications table
Medical: Flu → COVID test → diagnosis changes · Self-driving: road clear → pedestrian detected → brake · Robotics: door open → camera says closed → re-plan · Loans: eligible → default history found → reversed.

---

# TOPIC 2 — Commonsense Reasoning

**Definition:** ability to use **general world knowledge** to make reasonable assumptions when information is incomplete.
"Ahmed dropped a glass cup onto concrete" → breaks, scatters, maybe injury — **none of it stated**, every human knows it. Greatest strength of humans, historic weakness of AI.

### Why so hard for AI (3 reasons + the bottleneck)
1. **Everywhere but never written down** — nobody writes "water is wet"; AI needs millions of such facts.
2. **Compositional** — combining many small facts at once (umbrella + rain stopped + protection unneeded).
3. **Full of exceptions** — direct link to Topic 1 (defeasible).
**Knowledge Acquisition Bottleneck:** manually teaching AI everything a child knows × billions of situations — 50+ years unsolved. (Same bottleneck as MYCIN in L3.)

**Logical vs Commonsense:** logical = strict rules, guaranteed ("Socrates is mortal" — certain). Commonsense = defeasible, probabilistic ("Ali entered a restaurant → wants food" — usually true, retractable).

### 5 types of commonsense knowledge ⭐ (mnemonic: **2P-2S-T** — Physical, Psychological, Spatial, Social, Temporal)
| Type | Covers | Example inference |
|---|---|---|
| **Physical** | objects, natural world | thrown glass bottle → may break |
| **Spatial** | location, containment | keys in drawer, drawer in desk → keys in desk |
| **Temporal** | order/timing | graduated 2020 → was a student before 2020 |
| **Social** | interactions, roles | visited doctor → health concern |
| **Psychological** | emotions, intentions | failed exam → feels disappointed |

**Ice cream principle:** "John left ice cream on the table for an hour" → melts. 3 silent facts combined (melts at room temp + hour is long + tables indoors). **Commonsense is compositional.**

### The 3 famous problems ⭐⭐ (know which is which!)
| Problem | The question it asks | Hook |
|---|---|---|
| **Frame Problem** (late 1960s) | how to represent effects of an action **without listing everything that stays the same** | robot Move(Box) — wall colour unchanged, ×1000s |
| **Qualification Problem** | how to list **all conditions for an action to succeed** | "key starts car"… unless dead battery, no fuel, wrong key… |
| **Yale Shooting Problem** | benchmark: **persistence through time** | Load → Wait → Shoot should = dead; early systems failed because gun didn't "stay loaded" through Wait |

⚠ Frame = what STAYS THE SAME · Qualification = what SILENTLY PREVENTS. Frame problem's modern fix = default persistence (Event Calculus, Topic 3) — everything stays the same unless a rule changes it (reuses Topic 1's default idea).

### Commonsense knowledge bases ⭐
| KB | Focus | Built by |
|---|---|---|
| **Cyc** | general logical facts ("humans need food") | Douglas Lenat / Cycorp — rich but slow/expensive |
| **ConceptNet** | everyday concept relations (Car →UsedFor→ Transportation) | MIT Media Lab |
| **ATOMIC** | events + social causes/effects (Ali helps → Ali kind, person grateful) | Allen Institute for AI |

**LLMs & commonsense:** GPT-style models learned it statistically from billions of examples — answer "ice cream melted" correctly. Open debate: true causal understanding or statistical shortcut? → motivates **Topic 5 Neuro-Symbolic**.

### Worked examples to remember
- **Winograd Schema:** "Councilmen refused demonstrators a permit because **they** feared violence" → they = councilmen (motive reasoning; grammar can't decide — swapping 'feared'→'advocated' flips the answer).
- **Object permanence:** ball rolls behind couch → still exists there (objects don't vanish; solids can't pass through solids). Self-driving: pedestrian behind parked van still exists!
- **Restaurant script:** "Maria ordered salmon, left a big tip" → infer seated, menu, served, paid; "she enjoyed it" = weaker default, retractable (Topic 1 link).

---

# TOPIC 3 — Temporal Reasoning

**Motivation:** "Ali is in Lahore" — true WHEN? Hospital AI: Stable 8AM, Critical 10AM, Recovering 1PM — without time, three "simultaneous truths" = contradiction.
**Static knowledge** (2+2=4, water boils 100°C — rarely changes) vs **Dynamic** (location, temperature, traffic — continuously changes). AI mostly deals with dynamic.
**Definition:** represent + reason about time-dependent info — what happened first? how long? did they overlap?

### Points vs Intervals
- **Point-based:** instants/ticks (timestamps, simple ordering). Wasteful for durations.
- **Interval-based (Allen):** whole duration = one object with start+end. Natural for meetings, classes, surgeries.

### Allen's Interval Algebra ⭐⭐ — James F. Allen, 1983, **exactly 13 relations**
| Relation (pair) | Meaning | Example |
|---|---|---|
| Before / After | one ends before other starts | lecture before exam |
| Meets / Met-by | one ends EXACTLY as other begins (no gap) | Class 1 (9–10) meets Class 2 (10–11) |
| Overlaps / Overlapped-by | share SOME time, not all | meeting overlaps phone call |
| Starts / Started-by | same start, different ends | runners starting together |
| Finishes / Finished-by | same end, different starts | two flights landing together |
| During / Contains | one entirely INSIDE the other | midterm during semester |
| Equals | identical intervals | same meeting time |

Count: 6 pairs × 2 directions + Equals = **13**.
⭐ Worked: maintenance [2–4 PM], outage [3–3:30 PM] → outage starts after AND ends before → **During** → no separate rescheduling needed, outage absorbed by planned downtime.
⚠ **Order in a story ≠ order in time** — reconstruct timelines from clues ("before that", "two months earlier") + **transitivity** (E1<E2, E2<E3 ⟹ E1<E3). Thesis example: text order E3,E2,E1 = exact reverse of true timeline.

### Events, States, Fluents ⭐
- **Event** = occurrence that CHANGES the state. 4 components: **name, time, participants, effect** (OpenDoor, 10AM, {Ali, Door}, door open).
- **State** = condition of the world at a time. **Fluent** = property whose value changes over time (DoorOpen, Temperature, Location(Ali), BatteryLevel).

### Situation Calculus vs Event Calculus ⭐
| | **Situation Calculus** (early) | **Event Calculus** (improved, current) |
|---|---|---|
| World modeled as | sequence of situations; each action S0→S1 | **events** that initiate/terminate **fluents** over time |
| Weakness / strength | large systems → situation explosion, expensive | answers "what was true at time T?" for ANY T |

⭐ **Heater worked example (know cold):** 6:00 heater ON (initiates fluent) · 6:45 window opened (**irrelevant to this fluent**) · 7:10 heater OFF (terminates). Query 7:00 PM → falls inside [6:00, 7:10] → **heater ON**. Method: identify fluent → which events initiate/terminate it → build timeline → check query time.

**Applications:** robotics (Pick→Move→Deliver order), healthcare (fever→medication→improvement causality; heartbeat→BP drop→arrest prediction), smart cities (traffic patterns). Same tools everywhere: events + fluents + intervals.

---

# TOPIC 4 — Knowledge Graphs

**Motivating chain:** UMT→Lahore→Pakistan→Asia ⟹ "UMT is in Asia" — **never stated, inferred by chaining relationships.**
**vs Databases:** tables store isolated records; indirect links need multi-step JOINs. KGs store **connections as first-class objects** — humans think in relationships.
**Evolution:** Rules (hard to scale) → Semantic Networks (limited expressiveness) → Ontologies (still manual) → **Knowledge Graphs** (graphs + ontologies + semantics + ML) — foundation of modern search.

**Definition:** structured representation where **entities (nodes)** are connected by **relationships (edges)**, optionally annotated with **attributes** (Name, Age, CGPA).
**Inheritance** (from semantic nets): Sparrow IS-A Bird IS-A Animal → Sparrow inherits Can-Move, Has-Wings — never stored directly.

### RDF triples (same S-P-O as L4)
(Marie Curie, bornIn, Warsaw) · (Marie Curie, wonAward, Nobel Prize in Physics) · (Nobel Prize in Physics, awardedIn, 1903). Every edge in the graph = one triple; uniform shape lets billions of facts from different domains link up.
⭐ **Sentence → triples skill:** "Steve Jobs co-founded Apple in Cupertino in 1976" = **3 triples**: (SteveJobs, coFounded, Apple) · (Apple, foundedIn, Cupertino) · (Apple, foundedYear, 1976). Why separate: each fact can be queried/corrected independently. Check completeness: every noun phrase appears in ≥1 triple.

### Ontology vs Knowledge Graph ⭐ (classic confusion — one-liner)
> **Ontology = the GRAMMAR of knowledge** (vocabulary, classes, rules); **KG = the actual knowledge** built with that grammar.
University ontology: Classes {Student, Professor, Course…}, Relations {StudiesAt, Teaches…}, Rules. Without it, "Student/Learner/Pupil" = unrelated concepts.
⭐ **Contradiction catching:** rule "only Person can be marriedTo" + triple (Lahore, marriedTo, Karachi) → both are class City → **domain/range violation flagged automatically** — validation that scales to billions of triples.

### Reasoning in graphs ⭐
- **Direct** knowledge = stored explicitly. **Inferred** = derived, mostly via **transitivity** (A→B, B→C ⟹ A→C).
- **Answering a question = walking a path:** Marie Curie →wonAward→ Nobel Physics →fieldOf→ Physics.
- **Multi-hop worked example:** (Ahmed, worksAt, TechCorp) · (TechCorp, locatedIn, Lahore) · (Lahore, locatedIn, Pakistan) → Ahmed works in **Pakistan**, 3 hops. ⚠ More hops = less certain → real systems drop a confidence score per hop.

### Embeddings ⭐
**KG embedding** = entity/relation → numerical **vector** (Ali = [0.3, 0.8, 0.2]) so ML can process the graph: **link prediction, recommendation, classification, similarity.** Ali & Ahmed both StudiesAt UMT → vectors close → "likely share interests" (never stored).

### KGs + LLMs (bridge to Topic 5)
LLMs **hallucinate** (predict probable words, don't verify). **KG = verified facts; LLM = fluent language.** Combined = accurate AND fluent.

**Applications:** Google search (person→hasAward/hasTeam), Amazon (product→associatedWith→product), LinkedIn/Facebook (Ali knows Ahmed knows Sara → suggest), Healthcare (Symptom→indicates→Disease→treatedBy→Medicine).

---

# TOPIC 5 — Neuro-Symbolic AI

### History (3 phases)
**1. Symbolic AI** 1950–1985 (logic + rules, expert systems) → **2. Machine Learning** 1980–2010 (learn patterns from data) → **3. Deep Learning** 2012–now (perception superhuman).
**MYCIN lesson (L3 callback):** rules worked but experts had to hand-write thousands + endless exceptions = **Knowledge Acquisition Bottleneck** → motivated the shift to learning from data.

### Symbolic vs Neural ⭐⭐ (THE comparison table)
| Feature | Symbolic AI | Neural Networks |
|---|---|---|
| Learns from data | ❌ No (manual rules) | ✅ Yes |
| Logical reasoning | ✅ Strong | ❌ Limited |
| Explainable | ✅ every step traceable | ❌ black box |
| Perception (images/speech) | ❌ Poor | ✅ Excellent |
| Knowledge representation | ✅ Excellent | ❌ Weak |
| Scalability | ❌ Difficult | ✅ Excellent |
| Data needs | works with small data | extremely data-hungry |
| Failure mode | brittle in unexpected situations | wrong on unusual logical combos |

> **Core insight:** humans BOTH learn (child recognizes cat) AND reason (all cats mammals ⟹ Tom is mammal). Neuro-symbolic = replicate the dual ability.

### Definition + pipeline ⭐⭐ (memorize the 6 boxes)
**Neuro-symbolic AI** = hybrid combining neural **pattern-recognition** with symbolic **rule-based reasoning** — advantages of both, weaknesses of neither.
> **Raw Data → Neural Network (perception) → Concept Extraction → Knowledge Representation → Symbolic Reasoning (logic) → Decision**

⭐ **Pool safety example:** image → NN detects → facts `isChild(X), isPool(Y), distanceNear(X,Y)` → symbolic rule `IF child AND pool AND near THEN alertRisk` → send alert.
**Principle: perception and reasoning are DIFFERENT JOBS** — splitting them makes each simpler, verifiable, debuggable.

### Explainability (XAI) ⭐
Loan rejected, customer asks why. NN: "confidence 94%" — useless. Symbolic: "income below threshold; credit score insufficient; debt high" — actionable. Critical in **healthcare, banking, defense, law, autonomous vehicles**; regulators demand explanations. Neuro-symbolic supports it naturally because the FINAL step is symbolic/traceable.

### LLM + KG + Reasoner pipeline
> **Question → LLM understands → KG retrieves facts → Reasoning engine verifies → Answer.**
Fixes hallucination: LLM for language, KG for facts, reasoner for consistency.

### Applications (neural part vs symbolic part) ⭐
| Domain | Neural | Symbolic |
|---|---|---|
| Healthcare | detect tumor | + history → risk assessment |
| Robotics | detect box | plan path, avoid collision |
| Autonomous vehicles | detect objects | apply traffic rules + safety constraints |
| Finance | detect fraud pattern | regulatory compliance + explain |
| Science | find patterns | apply known laws to filter |

### 3 worked examples (know the mechanism)
1. **Visual QA:** "red object larger than the blue cube?" NN outputs facts (color/shape/size per object) → parse question to logical query → reasoner finds Object A satisfies both → "Yes" + cites facts. Pure NN fails on unusual logical combinations.
2. **Constraint injection:** NN gives 55% to a medically impossible diagnosis (male + female-anatomy disease) → hard rule forces P=0 → probability redistributed among valid options. Symbolic rules = **safety net statistics can't provide**.
3. **KG-embedding recommendation:** graph gives explainable candidates (MovieB via genre, MovieC via actor) → NN embeddings rank them by learned patterns → recommend MovieC, explanation still symbolic ("shares ActorX with a movie you watched").

**AGI note:** many researchers believe neither approach alone reaches AGI — real intelligence needs learning + reasoning together.

---

## ⭐ MOST-LIKELY EXAM QUESTIONS (priority order)
1. **CWA vs OWA** — table + library/KG example + risk of each.
2. **Tweety/Polly retraction walkthrough** — belief after each fact, why no contradiction.
3. **Nixon Diamond** — why undetermined; why guessing is dangerous.
4. **Allen's relation identification** — given two intervals, name the precise relation (During/Meets/Overlaps trap).
5. **Event Calculus fluent tracking** — heater-style: state at a time never directly mentioned.
6. **Frame vs Qualification vs Yale Shooting** — define, distinguish, and why each matters.
7. **Sentence → triples + multi-hop path** (Steve Jobs / Ahmed-TechCorp style).
8. **Symbolic vs Neural comparison** + why combine (design a neuro-symbolic system for domain X: name neural job, extracted facts, symbolic rule, decision).
9. **Ontology vs KG** one-liner + domain/range contradiction catching (Lahore marriedTo Karachi).
10. **TMS** — beliefs/justifications/dependencies + Road A walkthrough.
11. **5 types of commonsense** — classify given examples.
12. **Timeline reconstruction** from out-of-order story + transitivity.

## QUICK MNEMONICS
- L5 arc = **Retract → Obvious → Time → Connect → Combine** (NMR, Commonsense, Temporal, KG, Neuro-Symbolic).
- Non-monotonic = **conclusions can SHRINK**; monotonic = only grow.
- Specificity Preference = **specific beats general** (penguin beats bird).
- CWA=**False**, OWA=**Unknown**; DB=closed, KG/web=open.
- TMS = **B-J-D** (Beliefs, Justifications, Dependencies) — floors & pillars.
- Commonsense 5 = **2P-2S-T** (Physical, Psychological, Spatial, Social, Temporal).
- 3 famous problems: **Frame=stays-same · Qualification=silent-preventers · Yale=persistence-through-Wait.**
- Allen = **13 relations** = 6 pairs + Equals (Before, Meets, Overlaps, Starts, Finishes, During + Equals).
- Event = **N-T-P-E** (Name, Time, Participants, Effect); fluent = property that changes over time.
- Neuro-symbolic pipeline = **Data → Perceive → Extract → Represent → Reason → Decide.**
- LLM+KG = **fluent + factual** (LLM talks, KG verifies).
