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

---

# LECTURE 4

## Part 1 — The Semantic Web

### 1.1 — The problem: a web that displays but doesn't understand

- HTML tells the browser **HOW to display** (bold, layout) — nothing about **WHAT it means**. Old search = **word matching**: "father of Albert Einstein" → pages containing "father" + "Einstein." It even matches "father of modern physics" — words can lie about meaning.
- **Semantic Web fix:** store machine-readable facts with relationships: `Hermann Einstein —FATHER OF→ Albert Einstein` → machine **follows the relationship** and answers directly: Hermann.
- **Definition: teaching computers to UNDERSTAND information, not just display it.**
- Daily proof: **Google Knowledge Graph, 500 billion facts** — "Eiffel Tower" info card (Paris, 330 m, Gustave Eiffel) with **no website click**. Signatures: linked data · direct answers · no click.

### 1.2 — Three generations of the web

| | Web 1.0 (1990s) | Web 2.0 (2004+) | Web 3.0 (today) |
|---|---|---|---|
| Nickname | The Library | Library + Café | Smart Library |
| You can | READ only | READ + WRITE | machines UNDERSTAND |
| Examples | 1998 uni homepage, 2001 catalogue | Facebook, YouTube, Wikipedia, WhatsApp, Reddit | Knowledge Graph, Siri, Wikidata |

- **Dividing questions:** users can write? no → 1.0. yes → 2.0+. Machines understand meaning & answer directly? → 3.0.
- ⚠ My trap (fell in it): **interactive ≠ semantic** — Reddit voting and YouTube = **2.0** (humans interacting), NOT 3.0. Test: "is a MACHINE understanding meaning, or are HUMANS just interacting?"

### 1.3 — The Layer Cake (7 layers, bottom-up)

XML/Unicode (alphabet) → **URI** (unique ID per thing) → **RDF** (facts as triples) → RDFS (basic types) → **OWL** (what categories mean + rules) → Rules/SWRL (IF-THEN) → Trust (believe it?).

- Course studies: URI, RDF, OWL (+ SPARQL queries on top).
- **Hook: URI names it · RDF states it · OWL rules it.** (URI = CNIC for web resources.)

### Part 1 quiz & activity — SOLVED (likely exam Qs)

**① HTML hospital directory = Semantic Web?** NO — HTML only controls display. Missing: RDF/OWL markup adding machine-processable meaning ("this string is a hospital name, this is a location"). A table humans can read ≠ facts machines understand.

**② App answers "which cardiologist in Lahore is free Saturday?" directly** → **Web 3.0**; two technologies: **RDF** (doctor/specialty/availability as linked triples) + **SPARQL/KG query engine** (pattern-matches triples instead of keyword-searching pages).

**③ Facebook & Wikipedia = which generation?** **2.0** — users create/share, but machines can't read meaning. To become 3.0: add **semantic markup (RDF tags)** so machines understand entities + relationships.

**④ Classification set:** static 1998 homepage **1.0** · Google direct answer **3.0** · YouTube **2.0** · Siri pharmacy **3.0** · 2001 read-only catalogue **1.0** · WhatsApp **2.0** · Wikidata RDF triples **3.0** · Reddit-style voting **2.0**.

### 🧠 REMEMBER (Part 1)

- **Semantic Web = understand, not display.** Old web matches words; semantic web follows relationships.
- Generations: **Library → +Café → Smart Library.** Machine understands meaning? → 3.0. **Interactive ≠ semantic** (YouTube/Reddit = 2.0).
- **URI names it · RDF states it · OWL rules it.**
- Google KG: **500B facts, direct answers, no click.**

## Part 2 — RDF & OWL

### 2.1 — RDF triples & URIs

- **RDF = Resource Description Framework** — W3C standard for facts as **(Subject, Predicate, Object)** — grammar of a sentence: Ali —studies→ ComputerScience. Draw all triples → RDF knowledge graph (nodes = things, arrows = relationships).
- **URI = CNIC for web resources:** `<http://dbpedia.org/resource/Lahore>` — globally unambiguous; lets facts from different sources link to the SAME thing.
- Object kinds: **resource** (a thing, gets its own arrows — ComputerScience) vs **literal** (raw value, dead end — 1985, 14,000,000).
- Predicates = clean camelCase relations (foundedYear), entities named exactly as given (FAST, not FAST_Lahore unless stated).

### 2.2 — Turtle syntax: 4 punctuation marks

- **`.`** ends a complete statement · **`;`** same subject, new predicate · **`,`** same subject+predicate, new object · **`@prefix`** = URI shortcut. (Turtle = Terse RDF Triple Language.)
- Logic: *how much am I repeating?* Nothing → `.` · subject → `;` · subject+predicate → `,`
- `FAST isA University ; locatedIn Lahore ; offers CS , AI .` = 4 triples, ONE period.
- ⚠ My traps: **never forget the final period** (statement invalid without it); comma = only object changes, semicolon = predicate changes too; don't abbreviate entity names (uni ≠ University).

### 2.3 — OWL: rules about facts

- **OWL = Web Ontology Language** (letters reordered to say "owl" — the wise bird). **RDF stores FACTS · OWL stores RULES about them** (the instruction manual).
- 4 class constructs: **subClassOf** (every child IS parent) · **disjointWith** (nothing can be both) · **equivalentClass** (two names, one class) · **unionOf** (member of at least one).
- Properties: **ObjectProperty** = thing→thing (worksAt) · **DatatypeProperty** = thing→value (age 25).
- 3 behaviours: **Transitive** (chains: ancestor) · **Symmetric** (both ways: marriedTo) · **Functional** (at most one value: biologicalMother).
- ⚠ My trap: sort first — sentence about CATEGORIES → class construct; about a RELATIONSHIP's behaviour → property behaviour. Cues: "cannot be both"=disjoint · "same class"=equivalentClass · "every X is a Y"=subClassOf · "chains"=Transitive · "both ways"=Symmetric · "exactly one value"=Functional.
- **SNOMED CT:** 350,000+ medical concepts (NHS) — drug treats ViralInfection ⟹ auto-known to help COVID-19 (subclass), no manual link.

### 2.4 — The Reasoner: two jobs

- **Job 1 INFER (entailment up the chain):** Tux isA Penguin + Penguin⊑Bird ⟹ Tux is a Bird, cannot fly. Simba: 3 free inferences from 1 fact.
- **Job 2 CATCH contradictions:** Tux also isA Eagle (FlyingBird, disjoint with NonFlyingBird) → **INCONSISTENCY flagged automatically** — a FEATURE: data validation at scale, before harm (hospitals).

### Part 2 worked examples — SOLVED (likely exam Qs)

**① FAST activity + BONUS graph:** (FAST_Lahore, rdf:type, University) · (FAST_Lahore, locatedIn, Lahore) · (FAST_Lahore, foundedYear, 1985) · (FAST_Lahore, offers, ComputerScience) · (FAST_Lahore, offers, AI) · (FAST_Lahore, enrollment, 5000). Graph: FAST_Lahore in the centre, 6 labelled arrows out (two `offers` arrows — one per object):

```
         University        Lahore
              ▲              ▲
        rdf:type        locatedIn
              \             /
  1985 ◄─foundedYear─ FAST_Lahore ─offers──► ComputerScience
              /             \
       enrollment          offers
             ▼                ▼
           5000               AI
```

Key: FAST_Lahore appears in ALL 6 triples (central node). `offers` appears TWICE = two separate arrows. Literals (1985, 5000) are dead-end values; University, Lahore, CS, AI are resources.

**② Ahmed the Surgeon:** Surgeon⊑Doctor⊑HealthcareWorker + Ahmed isA Surgeon ⟹ reasoner infers **Ahmed is a Doctor AND a HealthcareWorker** — entailment up the chain, no extra triples written.

**③ Robot99:** isA Student AND isA Teacher + Student disjointWith Teacher → **INCONSISTENCY ERROR flagged** — useful: catches data-entry mistakes automatically (maybe he's a TA); that's the whole point of disjointWith.

**④ OWL reasoning set:** Simba (Lion⊑Mammal⊑Animal⊑LivingThing) → 3 auto-inferences · Tesla isA Car AND Bicycle (disjoint) → inconsistency · Wheel isPartOf Engine isPartOf Car (transitive) → **Wheel isPartOf Car** auto-added · Ali isMarriedTo Fatima (symmetric) → **Fatima isMarriedTo Ali** auto-added.

### 🧠 REMEMBER (Part 2)

- **RDF = Resource Description Framework = facts (S-P-O) · OWL = Web Ontology Language = rules about facts · URI = CNIC.**
- Turtle: **`.` ends · `;` new predicate · `,` new object · @prefix shortcut** — never forget the final period.
- Class constructs vs property behaviours — categories vs relationships. Cue words: both/same/every-X-is-Y vs chains/both-ways/one-value.
- **Reasoner: INFERS up subclass chains + FLAGS disjoint violations — automatic, at scale, a feature.**

## Part 3 — SPARQL

### 3.1 — Pattern matching + 4 query types

- **SPARQL** = query language for RDF graphs. **`?x` = a variable, a blank to fill**; the engine returns every combination filling ALL blanks simultaneously.
- WHERE patterns are **AND-ed** — each is a complete S-P-O triple with `?blanks`.
- 4 types: **SELECT** → table ("find all") · **ASK** → TRUE/FALSE ("does one exist?") · **CONSTRUCT** → new triples from old · **DESCRIBE** → all triples about one resource.
- ⚠ **ASK-vs-SELECT trap:** question ends in "yes or no" → ASK. `ASK { :DrKhan :taught :AI101 . }` — SELECT is wrong because you want existence, not data.

### 3.2 — FILTER, OPTIONAL, COUNT

- **FILTER = sieve** for conditions: `FILTER(?rating > 8.6)`. Cue: above/below/greater → FILTER.
- **OPTIONAL = left join:** without it, a missing value kills the WHOLE row (AND-ed patterns). `OPTIONAL { ?p :bloodType ?bt }` → all patients appear, cell empty if unrecorded. Cue: "include even if missing." (Query-level echo of missing ≠ false.)
- **COUNT/GROUP BY/ORDER BY:** `SELECT ?dir (COUNT(?movie) AS ?n) WHERE {…} GROUP BY ?dir ORDER BY DESC(?n)` → tallies per director, sorted.
- ⚠ My traps: every pattern = full S-P-O (subject seat ≠ predicate word); ONE WHERE block only — FILTER lives inside it; the number keyword is FILTER, not a second WHERE.

### Part 3 worked examples — SOLVED (likely exam Qs)

**① Nolan movies [SELECT]:** `SELECT ?movie WHERE { ?movie :director :Nolan . }` → Inception, Interstellar.

**② Rating > 8.6 [FILTER]:** `SELECT ?movie ?rating WHERE { ?movie :rating ?rating . FILTER(?rating > 8.6) }` → Inception (8.8).

**③ Any SciFi? [ASK]:** `ASK { ?m :genre :SciFi . }` → TRUE.

**④ Director tallies [COUNT]:** `SELECT ?dir (COUNT(?movie) AS ?n) WHERE { ?movie :director ?dir . } GROUP BY ?dir ORDER BY DESC(?n)` → Nolan 2, Scott 1.

**⑤ CS students CGPA>3.5:** `SELECT ?student WHERE { ?student isA :Student . ?student enrolled :CS . ?student cgpa ?cgpa . FILTER(?cgpa > 3.5) }` — 3 AND-ed patterns + FILTER.

**⑥ Blood types incl. missing [OPTIONAL]:** all patients listed; Sara (none recorded) keeps her row, cell empty — without OPTIONAL her row is absent.

### 🧠 REMEMBER (Part 3)

- **? = blank to fill; WHERE patterns AND-ed; each pattern a full S-P-O.**
- **SELECT table · ASK yes/no · CONSTRUCT new triples · DESCRIBE everything.**
- **Numbers → FILTER · "even if missing" → OPTIONAL (left join) · "how many per" → COUNT/GROUP BY.**

## Part 4 — Probability & Bayes' Theorem

### 4.1 — Why classical logic fails + the 3 probability rules

- Classical logic = **light switch** (TRUE/FALSE only). Real world needs a **dimmer** (0→1). Five breakers: **Missing info** (car not in DB ≠ no car) · **Uncertain data** (thermometer ±0.5°) · **Conflicting rules** (penguins) · **Vague words** (is Lahore "big"?) · **World changes** (facts expire). Fix: **Probability** for uncertainty (this part), **Fuzzy** for vagueness (Part 5).
- **Rule 1 Complement:** P(NOT A) = 1 − P(A) — everything totals 1.0. P(rain)=0.7 → P(no rain)=0.3.
- **Rule 2 AND (independent):** P(A AND B) = P(A) × P(B) — both must happen: heads twice = 0.5×0.5 = 0.25.
- **Rule 3 Conditional:** P(A|B) = prob of A GIVEN B — **the | shrinks the universe** to only-B, re-count inside: P(Ace|Spade) = 1/13 (52→13 cards) · P(6|even) = 1/3 (universe → {2,4,6}).

### 4.2 — Bayes' Theorem: starting guess → news → updated guess

**What Bayes IS:** you do it daily. "Pakistan wins today?" — *"50-50"* (**starting guess**). News: Babar injured. — *"30% now"* (**updated guess**). Your gut moved the number when news arrived. **Bayes = the calculator that does that movement with exact numbers.**

```
             P(E|H) × P(H)                    A
P(H|E)  =  -----------------   =   -----------------      (P(E) = A + B)
                 P(E)                     A + B
```

| Symbol | Fancy name | Real meaning |
|---|---|---|
| H | Hypothesis | the sentence in question: "THIS item is defective" |
| P(H) | Prior | **starting guess** — before any news ("2% are defective" → 0.02) |
| E | Evidence | **the news** — what you saw: flag / positive test / word FREE |
| P(E\|H) | Likelihood | the test's spec: signal rate among the YES-group |
| P(H\|E) | Posterior ★ | **updated guess** — after the news = THE ANSWER |

**How to compute — groups of 100 + two multiplications:**

1. Split 100 items into **YES-group** ("2% defective" → 2 items) and **NO-group** (98).
2. Signal rate per group: "flags 98% OF DEFECTIVE" → YES rate 0.98 · "wrongly flags 5% OF GOOD" → NO rate 0.05.
3. **A = YES-group × its rate** = 0.02×0.98 = 0.0196 (real alarms) · **B = NO-group × its rate** = 0.98×0.05 = 0.049 (false alarms).
4. **Updated guess = A/(A+B)** = 0.0196/0.0686 ≈ **28.6%** — "of all alarms, what share is real?"

**Reading rules:**

- **THE OF-RULE: the noun after "OF" goes AFTER the bar.** "80% of CHEATERS" → P(alert|cheater). Works always — even exposes sentences that are *already* the answer ("of all triggered bags, 12% banned" = P(banned|triggered) = already an updated guess, no Bayes needed).
- **Direction trap:** "beeps for 90% of expired" ≠ "90% of beeps are expired" — first is the spec (given), second is the updated guess (computed: it came out 61%, not 90!).
- **Chaining:** today's updated guess = tomorrow's starting guess (2nd test, Gmail's words).

### 4.3 — Worked calculations + the Base-Rate Effect (⭐ guaranteed exam question)

**Model layout (memorize the SHAPE — numbers change per question):**

```
Given: 1% have disease · test catches 95% of sick · 10% false alarms

Step 1 — LIST:    P(D) = 0.01     P(noD) = 0.99
                  P(+|D) = 0.95   P(+|noD) = 0.10
Step 2 — P(+):    A = 0.01 × 0.95 = 0.0095   (real alarms)
                  B = 0.99 × 0.10 = 0.099    (false alarms)
                  P(+) = A + B = 0.1085
Step 3 — ANSWER:  P(D|+) = A/(A+B) = 0.0095/0.1085 ≈ 8.76%
Interpret: despite a positive test, only ~9% likely sick — base-rate effect.
```

**Base-Rate Effect in plain English (the "explain WHY without calculating" question):** test 1 crore people. Sick 1% = 1,00,000 → 95% caught = **95,000 real positives**. Healthy 99,00,000 → 10% wrongly flagged = **9,90,000 false positives**. Only 95,000 of 10,85,000 positives are real → 8.76%. **A huge healthy group × a small false-alarm rate makes far more false alarms than the tiny sick group makes real ones. Rarity beats accuracy.**

**All five solved examples (same recipe every time):**

| Question | Starting guess | A | B | Updated guess |
|---|---|---|---|---|
| Disease (slides) | 1% | .0095 | .099 | **8.76%** |
| COVID (slides' activity) | 5% | .045 | .076 | **37.2%** |
| Spam "FREE" (slides' quiz) | 40% | .32 | .03 | **91.4%** |
| Factory scanner (my practice) | 2% | .0196 | .049 | **28.6%** |
| Cheating proctor (my practice) | 5% | .04 | .057 | **41.2%** |

- Feel the pattern: **rarer thing → lower updated guess** (1%→8.76% vs 40%→91.4%). Sanity-check every answer against it.
- Why COVID 37.2% > disease 8.76%: base rate 5% vs 1% — less rare → positives more meaningful.
- Follow-up they love: "what next?" → **second confirmatory test** — 37.2% becomes the new starting guess → two positives ≈ 92%+. Gmail: chains 100+ words → 99.9%.

### 4.4 — Bayesian Networks: the burglar alarm

```
Burglary (0.001)   Earthquake (0.002)
        \             /
         ▼           ▼
           🔔 ALARM          CPT: B+E→0.95 · B→0.94 · E→0.29 · neither→0.001
         /           \
        ▼             ▼
   John Calls     Mary Calls
```

- **Bayesian network = map of cause→effect arrows + probabilities.** Each node's **CPT (Conditional Probability Table)** comes **from domain experts — given, never calculated by you.**
- **FORWARD (predictive) = cause→effect, WITH the arrows — multiply along the path:** P(John calls | burglary) = 0.94 × 0.90 ≈ **85%**.
- **BACKWARD (diagnostic) = effect→cause, AGAINST the arrows — that's Bayes:** John called → P(burglary) 0.001 → **1.67%**; Mary also calls (updated guess becomes new starting guess) → **28.4%**.
- ⚠ Direction trap: "John calls — burglary?" = **BACKWARD**, and P(burglary) **INCREASES** (evidence supports its cause) — but stays far from certain (28.4%, not 90%). Don't invent hop numbers — read each from the given CPT.

### ⚠ My traps — in plain words (re-read tomorrow morning)

- **2% = 0.02, NOT 0.2.** Percent means "out of 100" → slide the decimal TWO places. 0.2 would be 20 out of 100 — ten times too big, ruins everything after it.
- **0.9 × 0.9 = 0.81, not 0.18.** Multiply digits (9×9=81), then count decimal places. Sanity check: two 90% events together should still be LIKELY — 0.18 screams "wrong."
- **After OF → after the bar.** I reversed it twice. "Alerts on 80% of cheaters": the 80% lives INSIDE the cheater group → P(alert|cheater). Writing P(cheater|alert)=0.80 claims the ANSWER before computing — the examiner's favourite wrong answer.
- **H is a sentence, P(H) is its number.** H = "this bike is expired" (a claim); P(H) = 0.15 (how likely the claim is, before news).
- **The dice answer isn't the card answer.** "Given even" → universe = {2,4,6} → P(6|even)=1/3. Recompute the shrunken universe fresh each time; don't parrot 1/13.
- **Forward multiplies along arrows; backward needs Bayes.** If you observed an EFFECT (call/flag/positive) and want the CAUSE — it's backward, use Bayes, and the cause's probability goes UP but rarely near certainty.

### 🧠 REMEMBER (Part 4)

- Light switch → **dimmer**. Five breakers: missing·uncertain·conflicting·vague·changing.
- 3 rules: **NOT = 1−P · AND = multiply · | shrinks the universe.**
- **Bayes = starting guess + news → updated guess** (cricket: 50% → Babar injured → 30%).
- **A = YES-group × its rate · B = NO-group × its rate · answer = A/(A+B).**
- **OF-rule: after "of" → after the bar.**
- **Base rate: rarity beats accuracy** (95,000 real vs 9,90,000 false).
- Networks: **forward = along arrows (multiply) · backward = Bayes; CPT from experts; updated guess chains.**

## Part 5 — Markov Models & Fuzzy Logic

### 5.1 — Markov Chains

- **Markov Property (one-liner worth marks):** *the next state depends ONLY on the current state — history is irrelevant.* Board game: token on square 34 — 5 or 50 moves to get there, doesn't matter. Chess: the current board already contains the history.
- **Transition matrix — ROW = where you are NOW (from), COLUMN = where you go (to):**

| From \ To | Sunny | Rainy | Cloudy |
|---|---|---|---|
| **Sunny** | 0.70 | 0.20 | 0.10 |
| **Rainy** | 0.30 | 0.40 | 0.30 |
| **Cloudy** | 0.50 | 0.30 | 0.20 |

- ⚠ **Every row sums to 1.0** — you must go somewhere (instant sanity check / find-the-error question).
- **THE finger-walk recipe (multi-step questions):**
  1. Each hop's number = **(current state's ROW → next state's COLUMN)**.
  2. **MULTIPLY hops within one story** — both must happen (AND rule).
  3. **ADD across stories** — any route counts (alternatives).
- **Worked example 1 (slides):** today Rainy → P(Sunny in 2 days)? Day-1 can be S/R/C → 3 stories:

| Story | Hop 1 | Hop 2 | Product |
|---|---|---|---|
| R→**S**→S | Rainy→Sunny = 0.30 | Sunny→Sunny = 0.70 | **0.21** |
| R→**R**→S | Rainy→Rainy = 0.40 | Rainy→Sunny = 0.30 | **0.12** |
| R→**C**→S | Rainy→Cloudy = 0.30 | Cloudy→Sunny = 0.50 | **0.15** |

  **Total = 0.21 + 0.12 + 0.15 = 0.48 = 48%**

- **Worked example 2 (my practice):** today Cloudy → P(Rainy in 2 days)? `C→S→R: 0.50×0.20=0.10` · `C→R→R: 0.30×0.40=0.12` · `C→C→R: 0.20×0.30=0.06` → **0.28 = 28%**
- **Single-path case:** exact route named ("Cart→Checkout→Buy in exactly 2 steps") → ONE story, just multiply: 0.5 × 0.7 = **35%**.
- Close every answer with: *only the current state was needed — history irrelevant.*

### 5.2 — Hidden Markov Models (HMM)

- **The umbrella prisoner:** man in a cell can't see the WEATHER (hidden state), only whether the guard carries an UMBRELLA (observation). Weather CAUSES the umbrella. **HMM = a real thing changing over time, seen only through its footprints.** Doctor: Healthy/Sick hidden, "feels Normal/Tired" observed. Siri: phonemes hidden, audio frequencies observed.
- **Two tables = two different arrows (every row sums to 1):**

**TRANSITION matrix (hidden → next hidden) — "how does the hidden thing move?"**

| From \ To | Healthy | Sick |
|---|---|---|
| **Healthy** | 0.70 | 0.30 |
| **Sick** | 0.40 | 0.60 |

**EMISSION matrix (hidden → what you see) — "what does the state show?"**

| State \ Observation | Normal | Tired |
|---|---|---|
| **Healthy** | 0.80 | 0.20 |
| **Sick** | 0.40 | 0.60 |

- **The two-row picture:**

```
Day:        Sun          Mon          Tue
HIDDEN:   Healthy ──?──▶  ?  ──?──▶   ?       ← blank row: the actual health (unknown)
                          │            │
                        emits        emits
                          ▼            ▼
SEEN:                   TIRED        TIRED    ← fixed, in ink
```

- **⭐ THE EYEBALL METHOD (exam tool — what the slides do):** for each observation, **pick the state that emits it most strongly** (read the emission table): Tired → Sick (0.60 beats 0.20) · Normal → Healthy (0.80 beats 0.40). Slides' sequence (Tired, Tired, Normal, Tired) → **Sick, Sick, Healthy, Sick** — no calculation needed. Grad-level closer: *"transition probabilities would refine borderline cases."*
- ⚠ **Spot-the-model trap:** ask *"is the state itself observable?"* Psychologist sees Engaged/Withdrawn/Tearful, never Depressed/Stable → **HMM**. Weather out the window → plain **Markov**.

### 5.3 — Fuzzy Logic: degrees of truth

- **The problem (vague words):** rule TALL = above 180cm → Ali 179.9 NOT TALL, Bilal 180.1 TALL — **0.2cm decided everything. Absurd.**
- **The fix — membership degree μ, between 0 and 1:** 160cm → μ(tall)=0.0 · 170 → 0.3 · 178 → 0.7 · 185 → 1.0. Smooth slope, no cliff; Ali at 179.9 → μ ≈ 0.85 "almost fully tall."
- **The three operations** (Ali: μ(tall)=0.7, μ(fast)=0.4):

| Operation | Formula | Ali | WHY |
|---|---|---|---|
| **AND** | **MIN**(μ₁, μ₂) | min(0.7, 0.4) = **0.4** | both needed → weakest link |
| **OR** | **MAX**(μ₁, μ₂) | max(0.7, 0.4) = **0.7** | either counts → strongest carries you |
| **NOT** | **1 − μ** | 1 − 0.7 = **0.3** | 70% tall forces 30% not-tall |

- **Solved (slides' quiz):** patient μ(young)=0.6, μ(healthy)=0.8 → AND = **0.6** ("60% both-young-and-healthy — limited by the weaker side, youth") · OR = **0.8** · NOT young = **0.4** (somewhat middle-aged).
- ⚠ **Fuzzy AND = MIN, NOT multiply** — multiplying is probability's AND. Keep the worlds apart: **probability = uncertainty about a fact** ("will it rain?") · **fuzzy = degree of a vague word** ("is it hot?").

### 5.4 — Fuzzy Inference System (F-R-A-D)

> **Crisp number in → F-R-A-D → crisp number out.** AC: 30°C in → 68% fan out.

1. **FUZZIFY** — input → membership degrees: 30°C → COOL 0.0 · WARM 0.5 · HOT 0.3.
2. **RULE EVALUATION** — **a rule fires at exactly the μ of its IF-part (just copy the number):** IF COOL→SLOW fires 0.0 · IF WARM→MEDIUM fires 0.5 · IF HOT→FAST fires 0.3.
3. **AGGREGATE** — clip each fired output shape at its strength (MEDIUM at 0.5, FAST at 0.3), merge into one shape.
4. **DEFUZZIFY** — **centroid (centre of gravity)** of merged shape → one number: **fan = 68%.**

- **Sanity rule: the output leans toward the strongest-firing rule.**
- **Solved at 22°C** (COOL 0.8, WARM 0.2, HOT 0.0): rules fire 0.8 / 0.2 / 0.0 → SLOW dominates → **fan ≈ 20–30%** — cool room, AC barely runs.
- **Solved at 35°C** (COOL 0.0, WARM 0.2, HOT 0.9): rules fire 0.0 / 0.2 / 0.9 → FAST dominates → **fan ≈ 85–90%.**
- **Cruise control (slides' activity):** error +5 km/h → ABOUT_RIGHT 0.5 + TOO_FAST 0.5 → HOLD and DECREASE tie → **slight throttle decrease**. Error −8 → TOO_SLOW 0.8 dominates → **strong increase**. **Further from target = stronger correction — like a human driver.**
- **Why ABS uses fuzzy (slides' quiz):** binary "IF locked THEN release" = jerky on-off braking; fuzzy graduated rules 14×/second = **smooth proportional pressure → shorter stops + steering control maintained.**
- Inventor: **Lotfi Zadeh, 1965** — rice cookers, camera autofocus, ABS, washing machines.

### Part 5 worked examples — SOLVED (likely exam Qs)

**① E-commerce Markov (slides' quiz):** from Cart: P(Checkout)=0.5; from Checkout: P(Buy)=0.7. Customer in Cart → P(Buy in exactly 2 steps) = single named path → multiply: **0.5 × 0.7 = 35%.** Markov property: only current state (Cart) matters — how they reached Cart is irrelevant.

**② Customer journey (slides' activity):** Browse→Cart = 0.30 (read straight from table). P(Browse→Cart→Checkout) = 0.30 × 0.50 = **15%**. Highest Leave rate = **Browse (50%)** → fix UX there (better search, recommendations, faster load); cutting Browse-Leave 50%→30% could nearly double conversion.

**③ Psychologist (slides' quiz):** can't see Depressed/Stable, observes Engaged/Withdrawn/Tearful → **HMM** — hidden states cause the observations; a Markov chain would be wrong because the state is unobservable.

**④ Chess (slides' quiz):** predict next move → only the **current board position** needed — **Markov property**; history is embedded in the present position.

**⑤ Patient fuzzy ops:** young=0.6, healthy=0.8 → AND 0.6 · OR 0.8 · NOT young 0.4 (solved in 5.3).

**⑥ AC at 22°C:** fires 0.8/0.2/0.0 → fan ~20–30%, mostly SLOW (solved in 5.4).

### 🧠 REMEMBER (Part 5)

- **Markov Property: next depends only on CURRENT state** — history irrelevant (square 34, chess board).
- Matrix: **row = from, column = to; every row sums to 1.**
- **Multiply within a story, add across stories.** Exact route named → one story, just multiply.
- **HMM = hidden states seen through footprints** (umbrella prisoner). Transition = hidden→hidden · Emission = hidden→seen. **Eyeball method: each observation points at its strongest emitter.**
- **Fuzzy: AND=MIN · OR=MAX · NOT=1−μ** — and fuzzy AND is NOT multiply.
- **F-R-A-D: Fuzzify → Rules fire at their IF-part's μ → Aggregate (clip+merge) → Defuzzify (centroid).** Output leans toward the strongest rule.
- Probability = uncertain FACT · Fuzzy = vague WORD.

---

## 🎯 THE BIG PICTURE (Lecture 4's closing slide — the quick selector)

**Tier 1 — storing CERTAIN knowledge:** facts → **RDF** triples · rules/meaning → **OWL** · queries → **SPARQL**.
**Tier 2 — reasoning under UNCERTAINTY:** update beliefs with evidence → **Bayes** · cause↔effect maps → **Bayesian Networks** · predict sequences from current state → **Markov/HMM** · vague words & smooth control → **Fuzzy**.

> Selector line for any exam scenario: **storing facts→RDF · rules→OWL · querying→SPARQL · updating belief→Bayes · causes+effects→BayesNet · sequences→Markov · hidden states→HMM · vague words→Fuzzy.**
