# KR Final Term — My Exam Sheet (approved lines)

> Final exam: Lectures 4 + 5 · Built concept-by-concept during study session 11 Jul 2026

---

# LECTURE 5

## Topic 1 — Non-Monotonic Reasoning ✅

### 1.1 — Monotonic reasoning (how classical logic works)

> **Adding information can only create new conclusions. It can never invalidate old conclusions.**

You learn something new → maybe you conclude something new. But old conclusions stay, always.

- Humans are mortal + Socrates is human → *Socrates is mortal.*
- Add "Plato is human" → NEW conclusion *Plato is mortal* added. The old one? Untouched.
- Add a thousand more facts — *Socrates is mortal* never goes away.

Like math: once 2+2=4 is proven, no new information can ever cancel it.

### 1.2 — Non-monotonic reasoning (how real life works)

> **Adding new information CAN cancel old conclusions.**

- Tweety is a bird → *Tweety flies.* New fact: penguin → conclusion **cancelled**.
- You weren't wrong — it was the best conclusion *with the information you had*. GPS reroutes, revised diagnoses, reversed loan approvals — all non-monotonic.

### 1.3 — The formula, in easy words

**Conclusions(K1) ⊆ Conclusions(K2)** = "everything I concluded before is still valid after adding new facts."
Holds → monotonic. A new fact cancels an old conclusion → formula breaks → **non-monotonic**.

### 1.4 — Defaults & conflicts

- **Default reasoning (Reiter):** birds fly *unless stated otherwise* — assume when nothing contradicts, stay ready to revise.
- **Specificity Preference:** specific rule beats general default — penguins-don't-fly beats birds-fly.
- **Polly problem:** "Polly is a bird" → tentative *can fly*; "broken wing" → 'unless' clause triggers → belief withdrawn → *cannot fly*. No contradiction — it was an assumption, not a proof.
- **Nixon Diamond:** Richard is both Quaker (→pacifist) and Republican (→not pacifist); neither rule more specific → tie unbreakable → **UNDETERMINED** — flag the conflict, don't guess (dangerous in medical/legal AI).

### 1.5 — CWA vs OWA — what does a system say about a fact it has NO record of?

- **CWA (Closed-World):** not written → **FALSE**. Justified only when the record is **complete by design** — bank statement, course registration, library loans. "If it were true, it would be in here."
- **OWA (Open-World):** not written → **UNKNOWN**. For records **incomplete by nature** — the web, knowledge graphs. Missing ≠ false.
- **Decision rule:** "is this record complete by design?" Yes → CWA. No → OWA.
- **Danger example:** no allergies recorded. CWA: "safe to prescribe" ⚠ deadly. OWA: "unknown — check first" ✅. **Absence of data ≠ absence of fact.**
- **Risks:** CWA → false negatives; OWA → less decisive.
- **CWA is non-monotonic:** add the missing record tomorrow → yesterday's "false" retracted.
- **Worked example (likely exam Q):** Library DB: "Ali borrowed Book A", "Sara borrowed Book B", no record about Ahmed. Q: under CWA, has Ahmed borrowed Book C? → **Steps:** ① only two loan facts exist, nothing about Ahmed ② CWA: not in KB → false, because the DB is assumed a complete record of ALL loans ③ conclusion: **Ahmed has NOT borrowed Book C — false, not unknown** ④ contrast: an OWA system (public web) would answer "unknown" instead. **Why it works:** everyday databases silently use CWA; it's non-monotonic — a new loan record tomorrow instantly retracts today's "false."

### 1.6 — Belief Revision — the umbrella cycle

**Knowledge → Inference → Conclusion → New Evidence → Revise → Updated Conclusion.** (Sunny → picnic → storm warning → cancelled.) Defaults, CWA and TMS all serve this cycle.

### 1.7 — TMS — the notebook of WHY

- TMS = a notebook: next to every belief, the AI writes the **reasons** it believes it.
- **One rule: a belief may only stay while its reasons are alive.**
- New fact kills a reason → open notebook → find beliefs listing that reason → **delete them** → beliefs depending on the deleted ones die too (chain reaction of un-believing).
- Payoff: only affected beliefs removed — **no re-scan of the whole knowledge base.**
- Analogy: **floor = belief, pillar = reason.** Break a pillar → floor falls → floors above fall.
- Walkthrough: "Road A is best" ← {no traffic + shortest}. Traffic reported → reason dead → **marked unsupported and retracted** → "leave at 5 PM" (resting on it) falls too → Road B recommended.

### ⚠ My traps (re-read before exam)

- I swapped the definitions first time: **MONO = only grows.** NON-mono = can take back.
- Write **Conclusions(K1) ⊆ Conclusions(K2)** — subset is on the CONCLUSIONS, not K1/K2.
- Polly has a broken **wing**, not leg.
- Nixon: the SYSTEM can't decide — answer is "undetermined / flag the conflict," not "Richard is neither."
- TMS doesn't "choose" — it **marks unsupported and retracts**, mechanically.

### 🧠 REMEMBER (Topic 1)

- **Monotonic:** adding info only creates new conclusions, never invalidates (2+2=4). **Non-monotonic:** new info CAN cancel old conclusions (GPS reroute).
- Formula: **Conclusions(K1) ⊆ Conclusions(K2)** — subset on CONCLUSIONS; holds = monotonic, breaks = non-monotonic.
- **Default:** "unless stated otherwise" · **Specificity:** specific beats general (penguin > bird) · **Nixon:** equal defaults → UNDETERMINED, flag don't guess.
- **CWA:** complete by design → missing = FALSE (bank statement) · **OWA:** incomplete by nature → missing = UNKNOWN (web). **Absence of data ≠ absence of fact.**
- **TMS notebook:** a belief stays only while its reasons are alive; reason dies → chain-delete, no full re-scan. Floors & pillars.

---

## Topic 2 — Commonsense Reasoning

### 2.1 — What it is

> **Using general world knowledge to draw reasonable conclusions when information is incomplete.**

- "Ahmed dropped a glass cup onto a concrete floor" → breaks, scatters, someone may get cut — **nobody stated any of it**; the knowledge is in the reader, not the sentence. A computer sees only words.
- "Ali entered a restaurant" → Ali is hungry, staff will attend to him — all **default assumptions, cancellable** (maybe he's only there for a meeting) → Topic 1's non-monotonic machinery running on world knowledge.
- Assumptions have **different strengths** — strong defaults (restaurant has tables) vs weak guesses (staff welcomes you) — weak ones get cancelled first.

### 2.2 — Why commonsense is hard for AI

Memory hook: **UNWRITTEN · COMBINED · EXCEPTIONS**

1. **Never written down** — all human text skips the obvious ("water is wet," "ice is cold"); an AI reading the whole internet still misses facts every child knows. Hand-coding billions of them = the **Knowledge Acquisition Bottleneck** (50+ years unsolved).
2. **Compositional — facts must be combined:** "ice cream on the table for an hour" → melted, needs 3 invisible facts together: melts at room temp + hour is long + tables are indoors. My coat example: coats are for warmth + outside cold + inside warm → coat comes off. Miss one fact → no conclusion.
3. **Full of exceptions** — every commonsense rule is defeasible (restaurant visitor may just be there for a meeting) → Topic 1's non-monotonic machinery.

### 2.3 — The 5 types of commonsense knowledge (2P-2S-T)

| Type | Answers | Example |
|---|---|---|
| **Physical** | how objects behave | milk out overnight → spoiled; glass bottle → breaks |
| **Spatial** | WHERE things are (containment chains) | remote in sofa pocket, sofa in lounge → remote in lounge |
| **Temporal** | WHEN — order & duration | graduated 2020 → student before 2020; movie before dinner |
| **Social** | people, roles, customs | attended janazah → likely knew the deceased |
| **Psychological** | feelings & intentions | smiled at result → probably passed |

- ⚠ **The type is decided by what the CONCLUSION talks about, not the event.** Janazah → "knew the deceased" = Social (custom/relationship); janazah → "feels grief" = Psychological (inside the head). Same event, different types.

### 2.4 — The Frame Problem

> **How can AI represent what an action changes WITHOUT explicitly listing everything it does NOT change?**

- Robot does `Move(Box)` → box position changed. But wall colour? Lights? Door? Logically the robot doesn't know they're unchanged unless told — a naive system needs "moving the box doesn't change the wall/door/lights…" × thousands of facts, **per action** → computationally impossible.
- **Fix: default persistence** — *everything keeps its state unless a rule explicitly changes it.* Topic 1's default reasoning recycled. The wall stays white by default; no statement needed. (Implemented in Event Calculus — Topic 3.)

### 2.5 — The Qualification Problem

> **How can we specify ALL the conditions required for an action to succeed?** (We can't — so assume normal conditions by default unless told otherwise.)

- "Turning the key starts the car" — unless: dead battery, no fuel, wrong key, damaged ignition… thousands of silent preventers humans ignore automatically.
- My flight example: "booking a ticket gets you on the flight" — unless booking unconfirmed, bad weather, cancelled, overbooked, visa/passport issue.
- ⚠ **Frame vs Qualification:** Frame looks AROUND the action (what stays the same after it); Qualification looks INSIDE it (what silently prevents it from working).

### 2.6 — The Yale Shooting Problem

- Benchmark: gun loaded, person alive. Actions: **Load → Wait → Shoot** → expected: dead.
- Early systems sometimes said *still alive* — nothing stated the gun **stayed loaded through Wait**, so "loaded" quietly un-happened.
- **Lesson: a true fact must PERSIST through irrelevant actions unless something explicitly changes it.** Motivated Topic 3 (Event Calculus).
- Exam line: **Frame = what stays the same · Qualification = what silently prevents · Yale = persistence through time.**

### 2.7 — Storing commonsense + the LLM twist

| Project | Stores | Built by | Hook |
|---|---|---|---|
| **Cyc** | millions of hand-coded logic facts ("humans need food") | Douglas Lenat / Cycorp | old, expensive, hand-written encyclopedia |
| **ConceptNet** | everyday relations graph: `Car →UsedFor→ Transportation` | MIT Media Lab | everyday-relations graph |
| **ATOMIC** | events + social causes/effects: helps elderly → kind, grateful | Allen Institute for AI | events-and-feelings base |

- **LLMs:** absorbed commonsense **statistically** from billions of texts (never hand-coded) — "ice cream on hot dashboard" → "melted."
- **Open debate (exam discussion Q):** true understanding of causation, or a powerful statistical shortcut? This debate motivates **Neuro-Symbolic AI (Topic 5)** — explicit symbolic reasoning on top of neural learning.

### Topic 2 worked examples — SOLVED (likely exam Qs)

**① Winograd Schema:** "Councilmen refused the demonstrators a permit because *they* feared violence." → **they = councilmen.** Steps: grammar stuck (both plural) → commonsense: permit-deniers act on safety caution + no group fears violence from itself → councilmen. **Proof grammar can't do it:** swap "feared"→"advocated" and the answer flips to demonstrators — needs reasoning about motives, not word statistics.

**② Ball behind couch (object permanence):** camera loses the ball → naive system: "unknown/no ball." Commonsense: ① objects keep existing unseen (object permanence, infants ~8 months) ② solids can't pass through solids → **ball is still behind/near the couch.** Stakes: self-driving car must assume the pedestrian behind the parked van still exists.

**③ Restaurant script:** "Maria… ordered the salmon… left a big tip." Recognize the restaurant SCRIPT (enter→seated→menu→order→served→eat→pay+tip→leave) → map stated events to slots → **fill unstated slots:** seated, menu, salmon prepared & served, she ate, she paid. Weak default: big tip → satisfied (cancellable — could be pity). Scripts fill enormous gaps from few facts; weak defaults cancel first.

### 🧠 REMEMBER (Topic 2)

- Commonsense = world knowledge filling gaps; conclusions are **cancellable defaults** (Topic 1 inside).
- Why hard: **UNWRITTEN · COMBINED · EXCEPTIONS** + Knowledge Acquisition Bottleneck.
- 5 types = **2P-2S-T** — decided by what the CONCLUSION talks about.
- **Frame = stays the same · Qualification = silently prevents · Yale = persistence through Wait.**
- Cyc = hand-coded · ConceptNet = relations graph · ATOMIC = events+feelings. LLMs = statistical commonsense → neuro-symbolic debate.
- Exam answers in the teacher's 4-step rhythm: **identify → apply principle → combine/check → conclude + why it works.**

---

## Topic 3 — Temporal Reasoning

### 3.1 — Why AI needs time

- "Ali is in Lahore" — true WHEN? Timeless storage (`InCity(Ali, Lahore)`) quietly drops the *when* → incomplete knowledge.
- **Hospital example:** Stable 8 AM · Critical 10 AM · Recovering 1 PM. No timestamps → all three true at once → **contradiction**. With timestamps → each fact bound to its own moment → no clash.
- **Static** knowledge rarely changes (2+2=4, water boils at 100°C) vs **Dynamic** changes over time (location, temperature, traffic, CGPA). Real AI lives in dynamic knowledge — robotics, healthcare, self-driving, finance.
- ⚠ My trap: slow-changing ≠ static — CGPA is **dynamic** (updates each semester). Test: *can it ever change?*, not *does it change fast?*
- **Temporal reasoning** = representing & reasoning about time-dependent info: what happened first, how long, did they overlap, what was true before/after.

### 3.2 — Representing time: Points vs Intervals

- **Point-based:** time = individual instants (clock ticks). Good for timestamps & simple ordering (bank transaction at 14:32, moment of birth).
- **Interval-based:** a whole duration = **ONE object with start + end**. Good for real activities — meetings, classes, surgeries, Ramadan, a 2-hour exam — because almost nothing interesting is instantaneous.
- **Why points fail for activities:** "Ali studied 8–11 PM" as points = a fact per tick: StudyingAt(8:00), StudyingAt(8:01)… = **180 duplicate facts** for one session (10,800 if ticking in seconds). As an interval = **one fact:** Studying(Ali, [8 PM → 11 PM]) — and "was he studying at 9:37?" is just: is 9:37 inside the interval? ✅
- Intervals unlock the rich questions — overlap? during? meets? — answered formally by **Allen's Interval Algebra: exactly 13 possible relations between any two intervals**.

### 3.3 — Allen's Interval Algebra (⭐ exam favourite)

James F. Allen, 1983: between ANY two intervals there are **exactly 13 possible relations** = 6 pairs (each + its reverse) + **Equals** (no reverse): 6×2+1 = 13.

| Relation | Test | Example |
|---|---|---|
| **Before / After** | A ends, **GAP**, B starts | azaan 6:00–6:05, jamaat 6:15 |
| **Meets / Met-by** | A's end = B's start, **zero gap** | Maghrib 6:30–6:45, dinner starts 6:45 |
| **Overlaps / Overlapped-by** | share some time, **each has private time** | meeting 3–4, call 3:45–4:15 |
| **Starts / Started-by** | same start, different ends | runners start together |
| **Finishes / Finished-by** | different starts, same end | two flights land together |
| **During / Contains** | one **fully swallowed** — starts after AND ends before | nap 3–4 inside loadshedding 2–5 |
| **Equals** | same start AND same end | match 7–9 = commentary 7–9 |

- **Method: 2 questions — compare the STARTS, compare the ENDS.** Nothing else matters (not how it "feels").
- ⚠ My traps (I fell in both): *Meets vs Before* → Meets = touching exactly, Before = gap. *Overlaps vs During* → Overlaps = private time on both sides; fully inside = During (tea break 9:30–9:45 in study 8–11 = During, even though it "feels" like an interruption).

### 3.4 — Events, States & Fluents

- **FLUENT = a property with a history** — its value can differ at different times: DoorOpen, Location(Ali), BatteryLevel, Temperature. Static facts (water boils at 100°C, 2+2=4) have no history → NOT fluents. *Fluents = 3.1's "dynamic knowledge," formalized.* Picture: a **whiteboard** holding the current value.
- **EVENT = the hand that rewrites a whiteboard** — a happening at a moment that changes a fluent's value. **4 components: Name · Time · Participants · Effect** (OpenDoor · 8:01 AM · {Ali, door} · DoorOpen: False→True).
- **STATE = reading all whiteboards at one moment** — the world's full snapshot at a time.
- **Rule:** between events, every whiteboard keeps its last written value — **nothing changes without an event** (persistence — Yale's lesson, formalized).
- **Worked mini-example:** "Ali booked a ticket at 3 PM" → Event = **BookTicket · 3 PM · {Ali, seat} · effect: fluent SeatAvailable: True → False.**
- ⚠ My traps: **participants never repeat the event's name** — they're the things involved ({Ali, seat}, not {Ali, ticketbook}). **Effect is written as a fluent's value change** (SeatAvailable: True→False), not as a loose phrase ("seat not available").

### 3.5 — Situation Calculus vs Event Calculus

- **Situation Calculus (early):** world = chain of snapshots; every action spawns a new situation (S0 →OpenDoor→ S1). Weakness: **situation explosion** — computationally too expensive.
- **Event Calculus (modern):** no snapshots — store **events + which fluent each initiates (switches ON) or terminates (switches OFF)**.
- **Lookup rule:** *a fluent is TRUE at time T if some event initiated it before T and no event terminated it in between.* Persistence built in (Yale's fix). Used in smart homes, healthcare monitoring, security, robotics.
- **"Fluent" in plain words = the status being tracked** — one changeable value on its own timeline; events rewrite only the whiteboard they target.
- **Worked example SOLVED (likely exam Q):** heater ON 6:00 · window opened 6:45 · heater OFF 7:10 — heater at 7:00? → ① status: HeaterOn ② initiated 6:00, terminated 7:10 ③ window event targets WindowOpen — **irrelevant** ④ 7:00 ∈ [6:00, 7:10] → **ON**. Power: any moment queryable, even one never mentioned.

### Topic 3 worked examples — SOLVED (likely exam Qs)

**① Timeline reconstruction:** "Hamid submitted Monday. Before that, supervisor reviewed 3 drafts. He'd started writing two months earlier." → events E1 write, E2 review, E3 submit; clues: "before that" E2<E3, "earlier" E1<E2; **transitivity** ⟹ E1<E3 → true order **E1→E2→E3** — exact reverse of sentence order. **Order in story ≠ order in time; extract clue words, apply transitivity.**

**② Allen factory case:** maintenance [2–4 PM], outage [3–3:30] → starts after AND ends before → **During** → scheduler flags nothing: outage absorbed by planned downtime.

*(③ heater Event Calculus — already solved in 3.5.)*

### 🧠 REMEMBER (Topic 3)

- Hospital contradiction → facts need **timestamps**; static vs dynamic; **fluent = property with a history**.
- Points = instants (timestamps) · **Intervals = one object with start+end** (activities).
- **Allen = exactly 13 relations** (6 pairs + Equals). Method: compare STARTS, compare ENDS. Traps: Meets = zero gap; fully inside = During, not Overlaps.
- **Event = Name·Time·Participants·Effect**, rewrites a fluent; state = all whiteboards at once; nothing changes without an event.
- **Event Calculus rule: true at T if initiated before T and not terminated in between.** Beats Situation Calculus (snapshot explosion).
- Story order ≠ time order → clue words + transitivity.

---

## Topic 4 — Knowledge Graphs

### 4.1 — Why graphs

- Chain hook: UMT→Lahore→Pakistan→Asia ⟹ "UMT is in Asia" — **never stated, derived by chaining relationships = multi-hop reasoning** (⚠ each extra hop = less certainty → systems attach dropping confidence).
- vs Databases: tables hold facts but the connection needs hand-written multi-step JOINs; **KG stores connections as first-class objects** — the link IS the data.
- **KG = entities (nodes) + relationships (edges) + attributes** (Name, CGPA).
- Evolution: Rules (can't scale) → Semantic Nets (limited expressiveness) → Ontologies (manual) → **KGs** (graphs+ontologies+semantics+ML).

### 4.2 — Triples & Ontology-vs-KG

- Every fact = **(Subject, Predicate, Object)**. Uniform shape → billions of facts linkable; each fact **correctable independently** (Apple founded in Los Altos? fix ONE triple).
- **Ontology = GRAMMAR** (classes, allowed relations, rules) · **KG = the actual knowledge** written in that grammar. Without ontology: Student/Learner/Pupil = 3 unrelated things.
- **Auto error-catching:** rule "only Person can be marriedTo" + `(Lahore, marriedTo, Karachi)` → both class City → **violation flagged automatically** — validation at billion-triple scale.
- ⚠ My traps: **don't invent facts the sentence never stated** (no bornIn!); **don't stuff details into predicates** (`captainInWC1992` ❌) — one fact, one triple: (ImranKhan, captained, PakistanTeam) · (PakistanTeam, won, WC1992) · (WC1992, heldIn, 1992).

### 4.3 — Reasoning, Embeddings, KG+LLM

- **Direct** = stored · **Inferred** = derived (transitivity). **Answering = walking a path** (Curie→wonAward→NobelPhysics→fieldOf→Physics).
- **Embeddings:** entity → numerical vector; same connection patterns → nearby vectors → **link prediction** — recommending facts never stored (Ali & Ahmed both →UMT → "share interests").
- **KG+LLM:** LLM hallucinates (predicts words, doesn't verify) · KG can't converse. **Pair: LLM = language, KG = verified facts.**
- Applications: Google answer cards · Amazon product→associatedWith→product · LinkedIn "people you may know" (Ali knows Ahmed knows Sara) · Healthcare Symptom→indicates→Disease→treatedBy→Medicine.

### Topic 4 worked examples — SOLVED (likely exam Qs)

**① Sentence→triples:** "Steve Jobs co-founded Apple in Cupertino in 1976" → (SteveJobs, coFounded, Apple) · (Apple, foundedIn, Cupertino) · (Apple, foundedYear, 1976). Check: every noun phrase in ≥1 triple, nothing added/lost; separate triples so each fact is independently correctable.

**② Multi-hop:** (Ahmed, worksAt, TechCorp) · (TechCorp, locatedIn, Lahore) · (Lahore, locatedIn, Pakistan) → "Which country does Ahmed work in?" → **3 hops → Pakistan** — answer derived, not looked up.

**③ Ontology catches error:** `(Lahore, marriedTo, Karachi)` vs rule "marriedTo needs Person on both sides" → both are City → **automatic violation flag** → reject or human review. Scales validation to billions of triples.

### 🧠 REMEMBER (Topic 4)

- **Chain = multi-hop reasoning**; derived ≠ stored; more hops = less certainty.
- **Ontology = grammar · KG = knowledge written in it**; ontology auto-catches violations.
- Triples: nothing invented, nothing stuffed into predicates.
- **Embeddings = vectors → link prediction** (unstored facts from patterns).
- **LLM talks, KG verifies.**

---

## Topic 5 — Neuro-Symbolic AI

### 5.1 — Two halves of AI

- History: **Symbolic 1950–85** (logic rules, expert systems) → **ML 1980–2010** (learn patterns) → **Deep Learning 2012–now** (perception superhuman).
- **MYCIN lesson:** hand-written rules worked but exceptions multiplied → **Knowledge Acquisition Bottleneck** → shift to learning from data.
- **THE comparison table — columns are mirror images:** Symbolic: reasons ✅, explains ✅, small data ✅, but can't learn ❌, poor perception ❌, brittle. Neural: learns ✅, perceives ✅, scales ✅, but black box ❌, weak logic ❌, data-hungry.
- Humans do BOTH (child learns cat + reasons Tom-is-mammal) → build machines with both.
- Age puzzle (Ali>Ahmed>Bilal): NNs learn **statistics, not logic** — simple transitivity needs many examples, still breaks on variants.

### 5.2 — Definition, pipeline, XAI

- **Definition:** hybrid combining neural pattern-recognition + symbolic rule-based reasoning — advantages of both, weaknesses of neither.
- **Pipeline (6 boxes):** Raw Data → Neural Network (perception) → Concept Extraction → Knowledge Representation → Symbolic Reasoning → Decision.
- **Pool safety example:** image → NN detects → facts `isChild(X), isPool(Y), distanceNear(X,Y)` → rule `IF child AND pool AND near THEN alertRisk` → alert.
- **Principle: perception and reasoning are DIFFERENT JOBS** — one giant NN doing both = unverifiable, undebuggable; split = simple, checkable parts.
- **XAI:** loan rejected — NN: "confidence 94%" (useless) vs symbolic: "income below threshold, credit score insufficient, debt high" (actionable). Regulators in healthcare/banking/defense/law/AV **demand explanations** — neuro-symbolic delivers because the final step is symbolic & traceable.
- ⚠ My trap: the extracted fact (hasTumor(X)) IS the concept-extraction output — don't count it twice, and don't skip the symbolic-rules stage (MRI → NN → hasTumor(X) → rules + patient history → risk decision).

### 5.3 — LLM+KG+Reasoner & applications

- **Anti-hallucination chain:** Question → **LLM understands** → **KG retrieves verified facts** → **Reasoner verifies consistency** → Answer. Fluent AND factual AND verified.
- Applications (neural | symbolic): Healthcare: detect tumor | history→risk · Robotics: detect box | plan path · AV: detect objects | traffic rules · Finance: spot fraud pattern | compliance+explain · Science: find patterns | apply known laws.
- **AGI note:** many researchers believe neither half alone reaches AGI — intelligence needs learning + reasoning together.

### Topic 5 worked examples — SOLVED (likely exam Qs)

**① Visual QA:** "Is there a red object larger than the blue cube?" → NN outputs facts (color/shape/size per object) → question parsed to logical query (∃X: red(X) AND size(X)>size(blueCube)) → reasoner searches facts → Object A matches → **"Yes," citing the exact facts.** Pure NN fails on unusual logical combos; split system handles questions far outside training.

**② Constraint injection:** NN gives 55% to a diagnosis impossible for a male patient → hard rule `IF male THEN P(DiseaseX)=0` forces it to zero, probability redistributed to valid options. **Data teaches statistics; only rules give guarantees.**

**③ KG-embedding recommendation:** graph gives explainable candidates (MovieB via genre, MovieC via shared ActorX) → NN embeddings rank them from millions of users' patterns → recommend MovieC, **explanation stays symbolic** ("shares ActorX with a movie you watched"). Symbolic = candidates + explanation; neural = ranking.

### 🧠 REMEMBER (Topic 5)

- Table = **mirror images**: symbolic reasons/explains, neural learns/perceives.
- **Pipeline:** Data → Perceive → Extract → Represent → Reason → Decide.
- **Perception and reasoning are different jobs** — split them.
- **LLM talks · KG verifies facts · Reasoner checks consistency.**
- **Rules give guarantees; statistics never do** (constraint injection).
- XAI: the final symbolic step makes decisions explainable → regulators satisfied.
