# KR Lecture 1 — Complete Cheat Sheet (Slides 1–30)

> Course: Knowledge Representation & Reasoning in AI
> Instructor: Dr. Husnain Ashfaq
> Coverage: Slides 1–30 (Foundations, Types of Knowledge, Propositional Logic intro)

---

## 1. Course Map

**5 Parts:** Foundations → Types of Knowledge → Propositional Logic → First Order Logic → Wumpus World

**KRR position in AI:** Storing facts + rules formally, applying logic to derive conclusions = "the brain's reasoning engine"

---

## 2. Foundational Hypothesis

**Newell & Simon (1976) — Physical Symbol System Hypothesis:**

> **Symbols + Rules = Intelligence**

- No need to simulate atoms or copy biological brain
- Justifies all symbolic AI: Prolog, Expert Systems, Knowledge Graphs, OWL/RDF
- Roman Urdu hook: *"Dimagh ki copy nahi chahiye, symbols aur rules kaafi hain"*

---

## 3. DIKW Pyramid (Data → Information → Knowledge → Wisdom)

| Level | Definition | Example |
|---|---|---|
| **D** Data | Raw, no context | "Vehicle count: 47" / "Temp=39.2" |
| **I** Information | Data + context, meaning landed | "Signal A is congested" / "Patient has high fever" |
| **K** Knowledge | Info + general rule | "IF vehicles>40 THEN extend green" / "IF fever+HR>100 THEN sepsis" |
| **W** Wisdom | Action + judgment + context/timing | "Extend green before Friday prayer" / "Start antibiotics before cultures return" |

**Wisdom = action + anticipation + cultural/local experience** (3 markers)

---

## 4. Real World → KB → Reasoning → Action Loop

```
REAL WORLD → KNOWLEDGE BASE → REASONING ENGINE → ACTION
```

**Standard format (admission example):**
- Real World: Ali — Grades=80, Test=60, FeesPaid=T
- KB Facts: Grades(Ali)=80, Test(Ali)=60, FeesPaid(Ali)=T
- KB Rules:
  - R1: IF Grades>60 AND Test>50 THEN Eligible
  - R2: IF Eligible AND FeesPaid THEN Admitted
- Reasoning (Modus Ponens):
  - Step 1 → Eligible(Ali)=T
  - Step 2 → Admitted(Ali)=T
- Action: System sends admission offer letter

---

## 5. Types of Knowledge — D / P / H / M

| Type | Asks | Giveaway phrase | Example |
|---|---|---|---|
| **Declarative** | WHAT is true? | Stored fact about entity | "Driver Ali rated 4.8" |
| **Procedural** | HOW to do it? | "Step 1 → Step 2", definite IF/THEN | "Insert PIN → check balance → dispense" |
| **Heuristic** | What USUALLY works? | *usually, often, typically* | "Friday evenings see 3× more rides" |
| **Meta** | About my knowledge? | "Our model X% accurate", "data is N min old" | "Our fraud model has 93% precision" |

### Critical Traps

| Phrase | Type | Why |
|---|---|---|
| "**Our model** is 90% accurate" | M | About own system |
| "**Users** convert 90% more" | H | About real-world pattern |
| "Data is 15 min old — refresh" | M | Self-aware about freshness |
| "Cancels >3 → flag account" (no "usually") | P | Deterministic |
| "Rain → surge **usually** 1.5×" | H | Word "usually" overrides IF/THEN |
| "Min balance rule: Rs 1,000" | D | Rule's *content* is a fact, not a process |

### Conversion Rule (Q5)

✅ **Declarative CAN be converted to Procedural**
- Declarative: "Tea = milk + sugar + boiled leaves" (WHAT)
- Procedural: "Boil water → add tea → add milk → add sugar" (HOW)
- AI systems use both: declare goal, then execute steps

---

## 6. Propositional Logic

### What is a proposition?
**A statement that is definitively TRUE or FALSE.**

### INVALID propositions (4 categories)
1. **Question** — "Is it raining?"
2. **Command** — "Close the textbook"
3. **Vague** — "She is tall", "x + y = 10"
4. **Paradox** — "This statement is false" ← Liar's Paradox

### 5 Connectives — Truth Tables

**AND (∧)** — both must be true

| P | Q | P∧Q |
|---|---|---|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | F |

**OR (∨)** — at least one true (inclusive)

| P | Q | P∨Q |
|---|---|---|
| T | T | T |
| T | F | T |
| F | T | T |
| F | F | F |

**NOT (¬)** — flip

| P | ¬P |
|---|---|
| T | F |
| F | T |

**IMPLIES (→)** — ⭐ ONLY false when P=T, Q=F

| P | Q | P→Q |
|---|---|---|
| T | T | T |
| T | F | **F** ← only false case |
| F | T | T |
| F | F | T |

**BICONDITIONAL (↔)** — true when same truth value

| P | Q | P↔Q |
|---|---|---|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | T |

---

## 7. Inference Rules

### Modus Ponens — THE core rule
```
P → Q       (rule)
P           (fact is TRUE)
─────
Q           (conclusion)
```

### Modus Tollens — backward version
```
P → Q
¬Q          (Q is FALSE)
─────
¬P          (therefore P is FALSE)
```

### Hypothetical Syllogism — chain rule (TAUTOLOGY)
```
(P → Q) ∧ (Q → R) → (P → R)
```
*All 8 rows of truth table = TRUE.*
**Plain English:** If A→B and B→C, then A→C directly.

### Disjunctive Syllogism (from Q9)
```
(P ∨ Q) ∧ ¬P  ⊢  Q
```
True only when P=F, Q=T.

---

## 8. Forward vs Backward Chaining

| Direction | Approach | Used in |
|---|---|---|
| **Forward** | Facts → Conclusions ("what can I derive?") | Production rule systems, Activity 5 |
| **Backward** | Goal → Facts ("is goal X true?") | Prolog default |

Both = Modus Ponens at every step. Different search direction.

---

## 9. Knowledge Base Structure

A KB = collection of logical sentences (facts + rules) the AI believes true.

**Example KB (University Admission):**
- Facts: MatricScore(Ali)=82, FSCScore(Ali)=79, TestScore(Ali)=88, Paid_Fee(Ali)=T
- Rules:
  - R1: IF Matric≥80 AND FSC≥70 AND Test≥85 THEN Eligible
  - R2: IF Eligible AND Paid_Fee THEN Admitted

---

## 10. Activity 5 — Home Security Inference Chain

**Facts:** DoorUnlocked=T, NightTime=T, OwnerAway=T

**5 Rules:**
- R1: MotionDetected → AlertTriggered
- R2: AlertTriggered ∧ NightTime → SendSMS
- R3: AlertTriggered ∧ NightTime → TurnOnLights
- R4: SendSMS ∧ OwnerAway → CallPolice
- R5: DoorUnlocked → MotionDetected

**Forward chain (5 steps):**
1. R5 → MotionDetected=T
2. R1 → AlertTriggered=T
3. R2 → SendSMS=T
4. R3 → TurnOnLights=T
5. R4 → CallPolice=T

**Final actions:** SendSMS · TurnOnLights · CallPolice

---

## 11. Limitation of PL → Why FOL Exists

**Problem:** "All students need a student ID"
- PL forces: NeedsID_Ali=T, NeedsID_Sara=T, ... (one per student!)
- Cannot write **single general rule** quantified over all students

**Solution:** First Order Logic introduces ∀ (for all), ∃ (there exists), variables, predicates → one rule covers all cases.

---

## 12. Q10 Inference Trace (Modus Ponens × 3)

Given: Rain=T
- Step 1: Rain → Flood ⇒ **Flood=T**
- Step 2: Flood → Evacuate ⇒ **Evacuate=T**
- Step 3: Evacuate → CallArmy ⇒ **CallArmy=T**

---

## Top 6 Lessons from Wrong Answers

1. **Data freshness/age/accuracy talk = META** (don't get fooled by action words)
2. **"Our model X%" = M, "Users do X% more" = H** (who is the subject?)
3. **Admission loop = 4 separate blocks + 2 rules + 2-step Modus Ponens chain**
4. **Declarative CAN be converted to Procedural** (tea/recipe example)
5. **Heuristic = general rule + "usually" + can fail** (not personal feeling)
6. **"This statement is false" = INVALID** (Liar's Paradox memorise)

---

## Top 5 Memorise-These (Quiz Gold)

1. **Newell & Simon, 1976 → Symbols + Rules = Intelligence**
2. **DIKW: Data (raw) → Info (meaning) → Knowledge (rule) → Wisdom (judgment+context)**
3. **D/P/H/M: WHAT / HOW / USUALLY / ABOUT-MY-KNOWLEDGE**
4. **Modus Ponens: P→Q, P ⊢ Q** — engine of every rule-based AI
5. **P→Q is FALSE only when P=T and Q=F** — golden implication rule

---

## Roman Urdu Hooks

| Concept | Hook |
|---|---|
| Representation Hypothesis | *"Dimagh nahi chahiye, symbols aur rules kaafi hain"* |
| Data | *"Sirf number, koi matlab nahi"* |
| Information | *"Number + matlab = samajh aaya"* |
| Knowledge | *"Agar X to Y — qaida"* |
| Wisdom | *"Tajurba + soch + waqt — pehle action lo"* |
| Declarative | *"Yeh sach hai"* |
| Procedural | *"Aise karna hai"* |
| Heuristic | *"Aksar chalta hai, lekin pakka nahi"* |
| Meta | *"Mere apne ilm ke baare mein"* |
| Modus Ponens | *"Agar P→Q hai aur P sach hai, toh Q bhi sach"* |
| Forward Chaining | *"Facts se aage badho"* |
| Backward Chaining | *"Goal se peechay aao"* |
| Tautology | *"Hamesha sach"* |

---

## Pending for Next Session (18:30–19:30)

- **Slide 30 — Q11** (Library System KB design)
- **Slide 30 — Q12 CHALLENGE** (Is P→(Q→P) a tautology? Build truth table; identify axiom)
- **Slides 31–57** (KR L1 second half — 4 knowledge types deep dive)

---

## Quick Recall Test (cover answers, recite)

1. Who said "Symbols + Rules = Intelligence" and when? → *Newell & Simon, 1976*
2. 4 levels of DIKW? → *Data, Information, Knowledge, Wisdom*
3. 4 types of knowledge? → *Declarative, Procedural, Heuristic, Meta*
4. Modus Ponens form? → *P→Q, P, ⊢ Q*
5. When is P→Q false? → *Only when P=T and Q=F*
6. Why does FOL exist? → *PL can't say "for all" / "there exists"*

If you can answer all 6 in 60 seconds → **quiz-ready.**
