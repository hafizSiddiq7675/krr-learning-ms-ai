# KR Lecture 1 — Session 2 Complete Notes (FOL)

> Session 2 coverage: First Order Logic (Part 4) — Slides 31–39
> Activity 6 (FOL Translation), Q13–Q16 Practice
> Total score: 203/240 = 85%

---

## Session Coverage

- **Slide 31:** Practice Q11 (Library KB) + Q12 (P→(Q→P) = Weakening axiom, tautology)
- **Slides 32–34:** FOL building blocks — Constants, Variables, Predicates, Functions, Quantifiers (∀, ∃)
- **Slide 35:** Socrates Proof (Universal Instantiation + Modus Ponens) + Activity 6
- **Slide 36:** Activity 6 Solution (8 university sentences in FOL)
- **Slide 37:** Activity 7 Hospital KB (reviewed only, sample answer)
- **Slides 38–39:** Practice Q13–Q16

---

# PART A — Complete Learning Material

## 1. What is FOL?

**FOL = First Order Logic** (also called Predicate Logic / First Order Predicate Calculus).

The most widely used formal language for representing knowledge in AI.

### Why FOL exists

PL (Propositional Logic) cannot say "for all" or "there exists." Forces one rule per object:
- NeedsID_Ali=T, NeedsID_Sara=T, NeedsID_Bilal=T... (thousands of rules!)

FOL fixes this with **one rule** covering all:
```
∀x: Student(x) → HasStudentID(x)
```

**One-liner:**
> FOL = PL + variables + quantifiers (∀, ∃) + predicates + functions.

**Roman Urdu:** *"PL har cheez ke liye alag rule maangti hai. FOL aik rule banao, sab pe lagao."*

---

## 2. The 5 Building Blocks of FOL

| Block | Meaning | Example |
|---|---|---|
| **Constants** | Specific things | Socrates, Lahore, CS301, Pakistan |
| **Variables** | Placeholders | x, y, course |
| **Predicates** | T/F properties or relations | Human(Socrates), Enrolled(Ali, CS301) |
| **Functions** | Return objects (not T/F) | FatherOf(Ali), CapitalOf(Pakistan) |
| **Quantifiers** | "For all" / "exists" | ∀, ∃ |

### Predicates by Argument Count

| Type | Args | Example | Meaning |
|---|---|---|---|
| **Unary** | 1 | Human(Socrates) | "Socrates is human" → T/F |
| **Binary** | 2 | Enrolled(Ali, CS301) | "Ali is enrolled in CS301" → T/F |
| **Ternary** | 3 | Scored(Ali, CS301, 88) | "Ali scored 88 in CS301" → T/F |

### Predicate vs Function — Key Difference

| | Returns | Example |
|---|---|---|
| **Predicate** | TRUE / FALSE | Human(Socrates) → T |
| **Function** | An object | FatherOf(Ali) → "Aslam" |

---

## 3. The Two Quantifiers — ∀ and ∃

### ∀ Universal Quantifier — "For All / For Every"

Pairs with **→** (implies).

**Examples:**
- `∀x: Human(x) → Mortal(x)` = "All humans are mortal"
- `∀x: Student(x) → HasStudentID(x)` = "Every student has a student ID"
- `∀x: Bird(x) ∧ ¬Penguin(x) → CanFly(x)` = "All non-penguin birds can fly"
- `∀x ∀y: Married(x,y) → Married(y,x)` = "Marriage is symmetric"
- `∀x: Employee(x) → HasSalary(x)` = "Every employee has a salary"

### ∃ Existential Quantifier — "There Exists / For Some"

Pairs with **∧** (and).

**Examples:**
- `∃x: Student(x) ∧ CGPA(x) > 3.9` = "There exists at least one student with CGPA > 3.9"
- `∃x: Doctor(x) ∧ Female(x)` = "There exists at least one female doctor"
- `∃x: Course(x) ∧ Credits(x) = 4` = "Some course has 4 credit hours"
- `∃x: City(x) ∧ Capital(x, Pakistan)` = "There is a city that is Pakistan's capital"
- `∃x: Student(x) ∧ CGPA(x) = 4.0` = "At least one student has a perfect GPA"

### The Critical Pairing Rule

| Quantifier | Connective |
|---|---|
| **∀** | **→** |
| **∃** | **∧** |

**Roman Urdu:** *"For all ke saath if-then. Exists ke saath AND. Mix mat karo."*

---

## 4. The Socrates Proof — Foundation of FOL Reasoning

The most famous example in logic, used for 2,000+ years.

### Knowledge Base
- **KB1:** ∀x: Human(x) → Mortal(x)   [All humans are mortal]
- **KB2:** Human(Socrates)              [Socrates is human]

### Goal
**PROVE:** Mortal(Socrates)

### Step 1 — Universal Instantiation
Take KB1: ∀x: Human(x) → Mortal(x)
Substitute x = Socrates:
```
Human(Socrates) → Mortal(Socrates)
```

### Step 2 — Modus Ponens
We have:
- Human(Socrates) → Mortal(Socrates)   [from Step 1]
- Human(Socrates)                       [from KB2]

Apply Modus Ponens:
```
Mortal(Socrates) ← PROVED ✓
```

### Why this matters

**Universal Instantiation + Modus Ponens = the foundation of all FOL reasoning.**

Same pattern works for:
- "All Pakistanis speak Urdu, Ali is Pakistani → Ali speaks Urdu"
- "All FAST students study CS, Sara is FAST student → Sara studies CS"
- "All employees with 5+ years are senior, Zara has 7 years → Zara is senior"

---

## 5. FOL Translation Master Patterns

| English Pattern | FOL Shape |
|---|---|
| "All X are Y" / "Every X is Y" | `∀x: X(x) → Y(x)` |
| "Some X is Y" / "There exists X" | `∃x: X(x) ∧ Y(x)` |
| "All X who Y do Z" | `∀x: X(x) ∧ Y(x) → Z(x)` |
| "Every X has a Y" | `∀x: X(x) → ∃y: Relation(y, x)` |
| "Some X does Y for all Z" | `∃x: X(x) ∧ ∀z: Z(z) → Does(x, z)` |
| "More than one X" | `∃x1, x2: X(x1) ∧ X(x2) ∧ x1 ≠ x2` |
| "More than N X" | use counting function: `Count(...) > N` |
| "No X without Y" | `∀: X(...) → (Action(...) → Y must hold)` |
| "X are Y that cannot Z" | `∀: X(...) → Y(...) ∧ ¬Z(...)` |
| "Some X does Y for all Z" | `∃x: X(x) ∧ ∀z: Z(z) → Does(x, z)` |

---

## 6. Threshold Cheat Sheet

| English | FOL |
|---|---|
| "more than N" | `> N` |
| "at least N" | `≥ N` |
| "exactly N" | `= N` |
| "fewer than N" | `< N` |
| "no more than N" | `≤ N` |

⚠️ "More than 3" = **> 3** (means 4+), NOT > 4.

---

## 7. Activity 6 — Full FOL Translations (University Domain)

**Predicates used:**
Student(x), Teacher(t), Course(c), StudiesCS(x), Teaches(t,c), Enrolled(s,c), Prerequisite(p,c), Passed(s,c), FailCount(s,n), OnProbation(s), Professor(p), Publishes(p), GetsIncrement(p), Counselor(c,s)

| # | English | FOL |
|---|---|---|
| 1 | All FAST students study CS | `∀x: Student(x) ∧ AtFAST(x) → StudiesCS(x)` |
| 2 | Some teachers teach more than one course | `∃t: Teacher(t) ∧ ∃c1, c2: Teaches(t,c1) ∧ Teaches(t,c2) ∧ c1 ≠ c2` |
| 3 | Every course has at least one student enrolled | `∀c: Course(c) → ∃s: Student(s) ∧ Enrolled(s,c)` |
| 4 | No student enrolls without prerequisite | `∀s,c: Enroll(s,c) → ∀p: Prerequisite(p,c) → Passed(s,p)` |
| 5 | Some student passed all 4-year courses | `∃s: Student(s) ∧ ∀c: FourYearCourse(c) → Passed(s,c)` |
| 6 | Fail >3 → probation | `∀s: Student(s) ∧ FailCount(s) > 3 → OnProbation(s)` |
| 7 | All publishing profs get increment | `∀p: Professor(p) ∧ Publishes(p) → GetsIncrement(p)` |
| 8 | Probation students have counselor | `∀s: OnProbation(s) → ∃c: Counselor(c, s)` |

---

## 8. Q13 — Bird FOL Translations (Reference)

| Sentence | FOL |
|---|---|
| (a) All birds can fly | `∀x: Bird(x) → CanFly(x)` |
| (b) Some birds cannot fly | `∃x: Bird(x) ∧ ¬CanFly(x)` |
| (c) Penguins are birds that cannot fly | `∀x: Penguin(x) → Bird(x) ∧ ¬CanFly(x)` |
| (d) Every animal that can't fly lives somewhere | `∀x: Animal(x) ∧ ¬CanFly(x) → ∃p: LivesAt(x, p)` |

---

## 9. Q14 — Inference KB (Pakistani/Lahori)

**KB:**
- R1: ∀x Pakistani(x) → SpeaksUrdu(x)
- R2: ∀x Lahori(x) → Pakistani(x)
- F1: Lahori(Ali)
- F2: Pakistani(Sara)
- F3: ¬Lahori(Sara)

**Inference traces:**

| Q | Answer | Trace |
|---|---|---|
| (a) Is Ali Pakistani? | YES | F1 + R2 instantiated at Ali ⇒ Pakistani(Ali) |
| (b) Does Ali speak Urdu? | YES | (a) + R1 instantiated at Ali ⇒ SpeaksUrdu(Ali) |
| (c) Does Sara speak Urdu? | YES | F2 + R1 instantiated at Sara ⇒ SpeaksUrdu(Sara) |
| (d) Is Sara Lahori? | NO | F3 directly: ¬Lahori(Sara) |

---

## 10. Q15 — Banking System FOL (Reference)

| Sentence | FOL |
|---|---|
| (a) Every account holder has valid ID | `∀x: AccountHolder(x) → HasValidID(x)` |
| (b) Some transactions are flagged | `∃t: Transaction(t) ∧ Flagged(t)` |
| (c) No minor opens savings without guardian | `∀x: Minor(x) → (CanOpenSavings(x) → ∃g: Guardian(x, g))` |
| (d) Flagged transactions notify owner | `∀t,x: Transaction(t) ∧ Flagged(t) ∧ Owner(x,t) → Notifies(t,x)` |

---

## 11. Q16 — Employee KB Inference

**KB:**
- R1: ∀x Employee(x) ∧ YearsExp(x) ≥ 5 → Senior(x)
- R2: ∀x Senior(x) ∧ HasProject(x) → GetsBonus(x)
- R3: ∀x GetsBonus(x) → TaxDeducted(x)
- F1: Employee(Zara), YearsExp(Zara) = 7, HasProject(Zara)
- F2: Employee(Bilal), YearsExp(Bilal) = 3, HasProject(Bilal)

**Answers:**
- (a) Zara bonus? **YES.** 7≥5 ⇒ Senior(Zara) ⇒ GetsBonus(Zara)
- (b) Zara tax deducted? **YES.** From (a) + R3 ⇒ TaxDeducted(Zara)
- (c) Bilal bonus? **NO.** 3<5 ⇒ ¬Senior(Bilal) ⇒ R2 cannot fire
- (d) New rule for Bilal: `∀x: Employee(x) ∧ HasProject(x) → GetsProjectAllowance(x)` (new benefit type, doesn't disturb existing rules)

---

# PART B — Mistakes I Made & How to Avoid Them

## Mistake #1: Wrong connective with ∃ (Q13b)

**Sentence:** Some birds cannot fly.

**My answer:** `∃b: Bird(b) → ¬CanFly(b)` ❌
**Correct:** `∃b: Bird(b) ∧ ¬CanFly(b)` ✓

**Trap:** Used → with ∃. Creates vacuous truth (any non-bird makes it true).

**Roman Urdu:** *"∃ ke saath hamesha ∧ lagao, → nahi. Agar koi cheez 'exist' kar rahi hai, toh dono conditions sach honi chahiye, isi liye AND."*

---

## Mistake #2: Wrong quantifier — "X are Y" = ∀, not ∃ (Q13c)

**Sentence:** Penguins are birds that cannot fly.

**My answer:** `∃p: Penguins(p) ∧ Bird(p) → ¬CanFly(p)` ❌
**Correct:** `∀x: Penguin(x) → Bird(x) ∧ ¬CanFly(x)` ✓

**Trap:** "Penguins are X" = universal claim about ALL penguins.

**Roman Urdu:** *"Jab kahein 'X are Y' (X log/cheezein Y hain) — yeh sab pe lagti hai, isi liye ∀. Agar 'some' nahi likha, hamesha ∀ samjho."*

**Trigger words:**
- "All / every / X are Y / X cannot..." → ∀
- "Some / a few / there exists / at least one" → ∃

---

## Mistake #3: "No X without Y" — recurring weak spot (Activity 6 #4 + Q15c)

**Sentence (#4):** No student can enroll without passing prerequisite.
**Sentence (Q15c):** No minor opens savings without guardian.

**My pattern:** Used ∃ or reversed direction → 3/10 both times ❌

**Correct pattern:**
```
∀variable: X(variable) → (Action(variable) → Y must hold)
```

**Roman Urdu:** *"'No X without Y' ka matlab hai — agar X ho raha hai, toh Y zaroori hai. Restriction hai, permission nahi. Hamesha ∀ aur nested → use karo. ∃ ya ¬ ka jaal mein mat phaso."*

**Mental translation:**
> "No X without Y" → "If X happens → Y must hold"
> X is the trigger, Y is the requirement.

---

## Mistake #4: "More than N" — use counting function, not N+1 variables (Activity 6 #6)

**Sentence:** If a student fails more than 3 courses → probation.

**My answer:** Listed only 3 course variables, no ≠ constraints ❌
**Correct:** `∀s: Student(s) ∧ FailCount(s) > 3 → OnProbation(s)` ✓

**Roman Urdu:** *"'More than N' ke liye counting function banao — N+1 variables aur ≠ likhne se behtar hai. FailCount(s) ek number return karta hai, Failed(s,c) sirf T/F."*

---

## Mistake #5: Off-by-one threshold (Activity 6 #6)

"More than 3" = `> 3` (which means 4+), NOT `> 4`.

**Roman Urdu:** *"'More than 3' = > 3 (matlab 4 ya zyada). > 4 ka matlab hota '5 ya zyada' — galat."*

---

## Mistake #6: "More than one X" — missed second variable + ≠ (Activity 6 #2)

**Sentence:** Some teachers teach more than one course.

**Correct:** `∃t, c1, c2: Teacher(t) ∧ Teaches(t, c1) ∧ Teaches(t, c2) ∧ c1 ≠ c2`

**Roman Urdu:** *"'More than one course' = do alag courses chahiye, isi liye c1 aur c2 introduce karo, aur c1 ≠ c2 likhna mat bhulna. Warna 'same course twice' bhi qualify ho jayega."*

---

## Mistake #7: "Every X has a Y" — used unary instead of ∃ (Activity 6 #8 + Q13d)

**Sentence:** Every probation student has a counselor.

**My answer:** `∀s: OnProbation(s) → CounselorAssigned(s)` (unary)
**Better:** `∀s: OnProbation(s) → ∃c: Counselor(c, s)` ✓

**Roman Urdu:** *"'Every X has a Y' ka matlab hai do quantifiers — outer ∀ aur inner ∃. ∃ se pata chalta hai Y kaun hai, sirf 'hasY(x)' likhne se Y ki identity gum ho jati hai."*

---

## Mistake #8: Q16d — Lowered threshold instead of new benefit

**Question:** Write a rule so Bilal also gets a benefit.

**My answer:** Lowered bonus threshold to 3 → breaks senior/bonus design ❌
**Better:** Create new benefit type `GetsProjectAllowance` ✓

**Roman Urdu:** *"Naya benefit add karna hai, purana threshold mat girao. Senior aur ProjectAllowance alag rakho — KB ka design clean rehta hai."*

**Design principle:**
> When extending a KB to cover an excluded case, **add new predicates/rules** — don't weaken existing thresholds.

---

# PART C — Roman Urdu Memory Hooks

| Concept | Hook |
|---|---|
| FOL purpose | *"PL har case ke liye alag rule, FOL aik rule sab pe"* |
| ∀ + → pairing | *"For all ke saath if-then"* |
| ∃ + ∧ pairing | *"Exists ke saath AND"* |
| Universal Instantiation | *"∀x P(x) ko aik specific cheez (Ali) pe lagao"* |
| Predicates vs Functions | *"Predicate sirf T/F deta hai, Function object deta hai"* |
| "X are Y" | *"∀ samjho — sab pe lagti hai"* |
| "Some X" | *"∃ samjho — kuch X exist karte hain"* |
| "No X without Y" | *"X ka condition Y, restriction not permission"* |
| "More than N" | *"Counting function use karo, variables list mat banao"* |
| "Every X has a Y" | *"Outer ∀ + inner ∃, dono chahiye"* |
| "Some X does Y for all Z" | *"Outer ∃ + inner ∀, dono nest karo"* |

---

# PART D — Strong Areas (What I Got Right)

✓ **Q14 (Pakistani/Lahori inference): 100%** — Universal Instantiation + Modus Ponens chain mastered
✓ **Q16 a/b/c: 100%** — KB inference, threshold checking, why rule fails
✓ **Activity 6 #1, #3, #5, #7: 100%** — basic ∀/∃ patterns nailed
✓ **Q13a, Q15a, Q15b: 100%** — clean unary patterns

---

# PART E — Final Tally

| Activity | Score |
|---|---|
| Activity 6 (8 sentences) | 67/80 = 84% |
| Q13 (Birds FOL) | 28/40 = 70% |
| Q14 (Inference) | 40/40 = 100% ✓ |
| Q15 (Banking FOL) | 31/40 = 78% |
| Q16 (Employee KB) | 37/40 = 92% |
| **Overall Part 4** | **203/240 = 85%** |

---

# PART F — Drill Plan Before Quiz

1. **Translate 5 sentences daily** using Master Patterns (Section 5)
2. **Especially drill "No X without Y"** — recurring weak spot
3. **Practice ∀ vs ∃ pairing** — write 3 examples each, no mistakes
4. **Build inference traces** — Fact + Rule → Universal Instantiation → Modus Ponens → Conclusion
5. **Memorise threshold cheat sheet** (>, ≥, =, <, ≤)

---

# PART G — Pending for Next Session

- **Slide 35 panel 2:** Class Activity 7 (Hospital KB) — sample reviewed only
- **Slides 40–55:** Part 5 — Wumpus World (full agent reasoning + FOL encoding)
- **Slide 55:** Practice Q17–Q20

---

# PART H — Quick Recall Test (60 sec)

Cover answers, recite:

1. What does FOL add to PL? → *Variables, predicates, functions, quantifiers (∀, ∃)*
2. ∀ pairs with which connective? → *→ (implies)*
3. ∃ pairs with which connective? → *∧ (and)*
4. What is Universal Instantiation? → *Substitute a specific constant for ∀x in a rule*
5. What does "No X without Y" become? → *∀ + nested →, restriction not permission*
6. "More than N" — best technique? → *Counting function, not N+1 variables*
7. Predicate vs Function? → *Predicate returns T/F, Function returns object*
8. The Socrates Proof in 2 steps? → *Step 1: Universal Instantiation. Step 2: Modus Ponens*

If you can answer all 8 in 60 seconds → **FOL quiz-ready.**
