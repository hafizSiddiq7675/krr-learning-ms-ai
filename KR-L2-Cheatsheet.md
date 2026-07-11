# KR Lecture 2 — Cheat Sheet (Quiz-Ready)

> Logical Inference + Forward & Backward Chaining
> Quiz: Sun May 03, 14:00

---

## 1. Logic Recap (from L1)

**Proposition** = declarative sentence, T or F. Not a question, command, paradox.

**5 Connectives:**
| Symbol | Name | True when |
|---|---|---|
| ¬P | NOT | P is false |
| P ∧ Q | AND | Both true |
| P ∨ Q | OR | At least one true |
| **P → Q** | IF-THEN | **FALSE only when P=T, Q=F** |
| P ↔ Q | IFF | Same truth value |

⚠ Implication = MOST important — every expert system rule is `P → Q`.

---

## 2. Entailment KB ⊨ α

**KB entails α** iff in every world where KB is true, α is also true.
α cannot possibly be false if KB is true.

---

## 3. Three Inference Rules

| Rule | Form | English |
|---|---|---|
| **Modus Ponens (MP)** | P → Q, P ⊢ Q | Rule + fact → conclusion |
| **Modus Tollens (MT)** | P → Q, ¬Q ⊢ ¬P | Deny result → deny cause |
| **Hypothetical Syllogism (HS)** | P→Q, Q→R ⊢ P→R | Chain two rules |

---

## 4. Three Types of Reasoning

| Type | Direction | Certainty | Use |
|---|---|---|---|
| **Deductive** | Rule + fact → conclusion | 100% guaranteed | Expert systems |
| **Inductive** | Many obs → general rule | Probable | ML, neural nets |
| **Abductive** | Observation → best cause | Best guess | Diagnosis, NLP |

**Test:**
- General rule given? → Deductive
- Many examples → new rule? → Inductive
- Observation → guessed cause? → Abductive

---

## 5. Expert Systems

**Definition:** AI program that captures knowledge + reasoning of a human expert in a specific domain.

### 3 Components

| # | Component | What it does |
|---|---|---|
| 1 | **Knowledge Base** | Stores facts + rules (IF-THEN). Has Working Memory. |
| 2 | **Inference Engine** | Scans rules. Fires those whose conditions are met. Derives new facts. |
| 3 | **Explanation Module** | Tells WHY a conclusion was reached. Traces which rules fired. |

### Production Rule Format
```
IF <condition1> AND <condition2> AND ... THEN <conclusion>
```

### Famous Expert Systems

| System | Year | Domain | Achievement |
|---|---|---|---|
| **MYCIN** | 1976 | Medicine | Diagnosed blood infections — 600 rules — performed as well as specialists |
| **XCON** | 1980 | Engineering | Configured DEC VAX computers — 10,000+ rules — saved $40M/year |
| **DENDRAL** | 1969 | Chemistry | Molecular structures from spectrometer data — first major AI success |

### Why Expert Systems > Neural Networks (for accountability)

- **KB explicit** — doctors can review/verify/update rules
- **Inference engine traceable** — each decision = specific rule chain
- **Explanation module** — outputs WHY, not just WHAT
- Neural networks = black box, can't explain in life-critical domains

---

## 6. First-Order Logic (FOL)

### Why FOL beats PL
PL needs one rule per object. FOL writes one rule for all.

### 5 Building Blocks
| Block | Example |
|---|---|
| Constants | Ahmed, London, 42 |
| Variables | x, y, z |
| Predicates | Fever(x), Loves(x,y) |
| Functions | FatherOf(x) |
| Quantifiers | ∀ (for all), ∃ (there exists) |

### ⚠ THE QUANTIFIER RULE (#1 EXAM TRAP)

**∀ pairs with → | ∃ pairs with ∧** — NEVER mix.

| ✓ Correct | ❌ Wrong |
|---|---|
| ∀x: Human(x) → Mortal(x) | ∀x: Human(x) ∧ Mortal(x) (says every object is BOTH) |
| ∃x: Student(x) ∧ Smart(x) | ∃x: Student(x) → Smart(x) (vacuous truth) |

### Translation Patterns

| English | FOL | Rule |
|---|---|---|
| Every X is Y | ∀x: X(x) → Y(x) | ∀ + → |
| Some X is Y | ∃x: X(x) ∧ Y(x) | ∃ + ∧ |
| No X can Y | ∀x: X(x) → ¬Y(x) | ∀ + → + ¬ |
| Every X with Y and Z does W | ∀x: X(x) ∧ Y(x) ∧ Z(x) → W(x) | compound condition |
| Only A does B | ∀x: B(x) → A(x) | "Only A does B" = "If B then A" |
| At least one X | ∃x: X(x) ∧ ... | "at least one" = ∃ |
| **No X without Y** | ∀x: X(x) → (Action(x) → ∃y: Y(y) ∧ Req(x,y)) | **two nested →** |
| More than N X | ∀x: Count(x) > N → ... | counting function (NOT N+1 vars) |
| Every X has a Y | ∀x: X(x) → ∃y: Has(x, y) | outer ∀ + inner ∃ |
| Some X does Y for all Z | ∃x: X(x) ∧ ∀z: Z(z) → Does(x,z) | outer ∃ + inner ∀ |

---

## 7. Horn Clauses

**Horn Clause** = rule with at most ONE positive conclusion (head) + any conditions (body).

### Format
```
Head ← Body1 ∧ Body2 ∧ ... ∧ BodyN
```

### Two types

| Type | Example |
|---|---|
| **Fact** (no body) | `Employed(Ahmed) ←` |
| **Rule** (head ← body) | `EligibleForLoan(x) ← Employed(x) ∧ AgeOver21(x) ∧ GoodCredit(x)` |

**Used by:** Prolog, CLIPS. Make reasoning efficient + decidable.

**Equivalent to IF-THEN:**
```
IF Employed(x) ∧ Over21(x) THEN Eligible(x)        ← IF-THEN
Eligible(x) ← Employed(x) ∧ Over21(x)              ← Horn
```

---

## 8. Forward Chaining (FC) — DATA-DRIVEN

**Starts with:** known facts.
**Asks:** "What new things can I derive?"
**Direction:** Data → Conclusions.

### Algorithm (4 steps)

1. **MATCH** — check every rule against working memory
2. **SELECT** — pick a rule whose conditions are all satisfied (conflict set)
3. **FIRE** — apply the rule, add conclusion as new fact in WM
4. **TERMINATE?** — goal reached → STOP. No new fact added → STOP. Else go to step 1.

### When to use
- Sensor monitoring (data arrives, derive consequences)
- Diagnosis when no specific goal
- "Given this, what follows?"

### FC trace template
```
Initial WM: {facts...}

Pass 1: R? checks → conditions ✓ → fires → new fact added
Pass 2: R? checks → conditions ✓ → fires → new fact added
...
Pass N: no rule can fire → STOP

Final WM: {original + derived facts}
Conclusion: ...
```

---

## 9. Backward Chaining (BC) — GOAL-DRIVEN

**Starts with:** a goal.
**Asks:** "What do I need to prove this?"
**Direction:** Goal → Facts.

### Algorithm

1. Start with goal `G`
2. Find a rule whose conclusion matches `G`
3. The rule's conditions become **sub-goals**
4. Recursively prove each sub-goal
5. If a sub-goal is a known fact → ✓
6. If all sub-goals proved → goal proved

### When to use
- Specific question: "Is X true?"
- Diagnosis: "Why was X recommended?"
- Prolog default

### BC trace template
```
Goal: G ?
   ↓ (Rule R: A ∧ B → G)
Sub-goals: A ? AND B ?
   A ? → fact ✓
   B ? → use rule R': C → B → sub-goal C ? → fact ✓
GOAL PROVED ⭐
```

---

## 10. FC vs BC Comparison Table (LIKELY QUIZ)

| Feature | Forward Chaining | Backward Chaining |
|---|---|---|
| **Direction** | Facts → Conclusions | Goal → Facts |
| **Trigger** | New fact arrives | Question asked |
| **Approach** | Data-driven | Goal-driven |
| **When to use** | Monitoring, no specific goal | Specific question to answer |
| **Complexity** | Can derive irrelevant facts | Focused on goal only |
| **Example use** | Sensor systems | Diagnostic queries (Prolog) |

**Both use Modus Ponens at every step. Different search direction.**

---

## 11. Worked Example — Smart Home (Problem 3)

**Rules:**
- R1: Nighttime ∧ NoMovement → HouseInactive
- R2: HouseInactive ∧ AllDevicesIdle → LowEnergyUsage
- R3: HighPrices ∧ LowEnergyUsage → RecommendEnergySaving
- R4: RecommendEnergySaving → ActivateEnergySavingSystem

**Facts:** Nighttime, NoMovement, AllDevicesIdle, HighPrices

### FC trace
```
P1: R1 fires → HouseInactive added
P2: R2 fires → LowEnergyUsage added
P3: R3 fires → RecommendEnergySaving added
P4: R4 fires → ActivateEnergySavingSystem added ⭐
P5: STOP
```

### BC trace (Goal: ActivateEnergySavingSystem)
```
Goal? → R4 → need RecommendEnergySaving
   → R3 → need HighPrices ∧ LowEnergyUsage
       HighPrices ✓ FACT
       LowEnergyUsage → R2 → need HouseInactive ∧ AllDevicesIdle
           AllDevicesIdle ✓ FACT
           HouseInactive → R1 → need Nighttime ∧ NoMovement
               Nighttime ✓ FACT
               NoMovement ✓ FACT
GOAL PROVED ⭐
```

---

## 12. Common Quiz Traps

1. **∃ with → instead of ∧** — vacuous truth (Mistake #1)
2. **"X are Y" = ∀** — not ∃ (Mistake #2)
3. **"No X without Y" needs nested →** — not flat ∧ (Mistake #3)
4. **"More than N" = counting function** — not N+1 variables (Mistake #4)
5. **"Every X has a Y" = outer ∀ + inner ∃** — not unary (Mistake #7)
6. **Confusing FC with BC** — direction matters (FC = data, BC = goal)
7. **"Expert system" ≠ a reasoning type** — it's a container; type is deductive
8. **AND in conditions** — ALL must be true to fire (one false = rule doesn't fire)

---

## 13. Quick Recall (60 sec — be quiz-ready)

1. 3 inference rules? → MP, MT, HS
2. 3 reasoning types? → Deductive (certain), Inductive (probable), Abductive (best guess)
3. 3 expert system components? → KB, Inference Engine, Explanation Module
4. ∀ pairs with? → →
5. ∃ pairs with? → ∧
6. Horn Clause format? → Head ← Body1 ∧ ... ∧ BodyN
7. FC direction? → Facts → Conclusions
8. BC direction? → Goal → Facts
9. FC algorithm 4 steps? → Match, Select, Fire, Terminate?
10. P → Q is FALSE when? → P=T, Q=F (only)

If you can recite all 10 in 60 sec → **L2 quiz-ready.**

---

## 14. Roman Urdu Hooks

| Concept | Hook |
|---|---|
| Modus Ponens | *"Rule + fact = conclusion"* |
| Modus Tollens | *"Result jhoot, toh cause bhi jhoot"* |
| Deductive | *"Qaida + fact = 100% jawab"* |
| Inductive | *"Bohot baar dekha, rule banaya"* |
| Abductive | *"Yeh dekha, sabab yeh hoga shayad"* |
| Expert System | *"KB + Engine + Explanation"* |
| ∀ + → | *"For all ke saath if-then"* |
| ∃ + ∧ | *"Exists ke saath AND"* |
| FC | *"Facts se aage badho, naye facts banao"* |
| BC | *"Goal se peechay aao, dekho kya chahiye"* |
| Horn Clause | *"Sirf ek conclusion, ulta arrow, Prolog ka format"* |

---

## 15. A4 Layout (handwrite this)

**FRONT:**
- Top-left: 5 connectives + P→Q golden rule
- Top-right: MP / MT / HS forms
- Middle-left: Reasoning types table (D/I/A)
- Middle-right: Expert System components (KB / IE / EM)
- Bottom: ∀/∃ pairing rule + 5 translation patterns

**BACK:**
- Top: FC algorithm (4 steps)
- Top-right: BC algorithm (recursive)
- Middle: FC vs BC comparison table
- Bottom: Smart Home worked example (FC + BC traces)

If it doesn't fit on one A4, you don't understand it yet — compress more.
