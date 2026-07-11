# KR Lecture 1 — Part 5: Wumpus World (Capstone Notes)

> Quiz: Sun May 03, 14:00. **Highest-leverage topic** — bundles propositional logic + MP + FOL in one problem.
> Slides 40–55 (Lecture 1.pdf).

---

## 1. The Setup (5.1)

**4×4 grid cave.** Introduced by Gregory Yob (1973), adopted by Russell & Norvig.

| Object | Effect |
|---|---|
| 🩸 Wumpus | Walk in = die |
| ⚠ Pits | Fall in = die |
| ⭐ Gold | Treasure to collect |
| ▶ Agent | Starts at (1,1), facing right |

**Agent is BLIND** — cannot see neighboring rooms, only senses current room.
**Goal:** Find gold, pick up, return to (1,1) alive.

---

## 2. The 5 Percepts (5.2)

| Percept | Meaning | Inference |
|---|---|---|
| **STENCH** | smell awful | Wumpus is **adjacent** (not here) |
| **BREEZE** | feel a draft | Pit is **adjacent** |
| **GLITTER** | something shines | Gold is **in THIS room** — grab! |
| **BUMP** | hit a wall | tried to step outside grid |
| **SCREAM** | terrible shriek | Your arrow killed the Wumpus |

**Key trap:** Stench + Breeze → adjacency. Glitter → exact room.

---

## 3. The 5 World Rules (5.3) — MEMORIZE

| # | Rule | Propositional | FOL |
|---|---|---|---|
| **W1** | Stench ↔ Wumpus adjacent | `S[x][y] ↔ (W[x-1][y] ∨ W[x+1][y] ∨ W[x][y-1] ∨ W[x][y+1])` | `∀x ∀y: Stench(x,y) ↔ ∃a ∃b: Adjacent(x,y,a,b) ∧ Wumpus(a,b)` |
| **W2** | Breeze ↔ Pit adjacent | `B[x][y] ↔ (P[x-1][y] ∨ P[x+1][y] ∨ P[x][y-1] ∨ P[x][y+1])` | `∀x ∀y: Breeze(x,y) ↔ ∃a ∃b: Adjacent(x,y,a,b) ∧ Pit(a,b)` |
| **W3** | Glitter ↔ Gold (same room) | `Glitter[x][y] ↔ Gold[x][y]` | `∀x ∀y: Glitter(x,y) ↔ Gold(x,y)` |
| **W4** | Death | `Dies ↔ Agent same room as Wumpus OR Pit` | `∀x ∀y: Agent(x,y) ∧ (Wumpus(x,y) ∨ Pit(x,y)) → Dies` |
| **W5** | Safety | `OK[x][y] ↔ ¬Pit[x][y] ∧ ¬Wumpus[x][y]` | `∀x ∀y: OK(x,y) ↔ ¬Pit(x,y) ∧ ¬Wumpus(x,y)` |

**Why FOL beats PL:** A 4×4 grid needs 16 propositional rules per concept (one per room). FOL writes ONE rule for any grid size.

---

## 4. The Standard Map (5.4)

```
Row 4 │ Wumpus(1,4)│ GOLD(2,4) │ Pit(3,4)  │           │
      │ [Stench]   │ [S+B]     │ [Breeze]  │ [Breeze]  │
Row 3 │ [Stench]   │ [Breeze]  │ Pit(3,3)  │ [Breeze]  │
Row 2 │ [Stench]   │           │ [Breeze]  │           │
Row 1 │ AGENT(1,1) │ [Breeze]  │ Pit(3,1)  │           │
      │  start     │           │ [Breeze]  │           │
```

**Memorize this layout** — it's the standard for every quiz problem.

---

## 5. The 7-Step Trace (5.5) — THE quiz target

Each step uses three forms in parallel: **Percept → Propositional → Modus Ponens → FOL → Decision.**

### Step 1 — at (1,1), Percept: NOTHING

- **Prop:** `¬B[1][1]=T`, `¬S[1][1]=T`
- **MP via W2:** `¬B(1,1) → ¬P(1,2) ∧ ¬P(2,1)` ⇒ both safe from pits
- **MP via W1:** `¬S(1,1) → ¬W(1,2) ∧ ¬W(2,1)` ⇒ both safe from Wumpus
- **KB update:** `OK(1,2)=T, OK(2,1)=T`
- **Decision:** Move to (2,1) — safe.

### Step 2 — at (2,1), Percept: BREEZE

- **Prop:** `B[2][1]=T` ⇒ Pit adjacent
- **Adjacents of (2,1):** (1,1) [OK], (3,1), (2,2)
- **Conclusion:** `Pit(3,1) ∨ Pit(2,2)` — uncertain!
- **KB update:** `PitPossible(3,1)=T, PitPossible(2,2)=T`
- **Decision:** RETREAT to (1,1). Don't risk either.

### Step 3 — at (1,2), Percept: STENCH (no breeze)

This is **the key reasoning step** — agent DEDUCES Pit and Wumpus locations without seeing them.

- **W1 at (1,2):** Stench ⇒ Wumpus adjacent. Options: (2,2), (1,3), (1,1)❌. So `W(2,2) ∨ W(1,3)`.
- **W2 at (1,2):** ¬Breeze ⇒ `¬Pit(2,2)`.
- **Combine:** From Step 2, `Pit(3,1) ∨ Pit(2,2)`. Now `¬Pit(2,2)` ⇒ **`Pit(3,1)=CONFIRMED`** ✓
- **Wumpus deduction:** From (1,3): Stench(1,3)→Wumpus in (1,4) or (2,3). But (2,3) gives no stench at (1,3) reach? — ⇒ **`Wumpus(1,4)=CONFIRMED`** ✓
- **KB update:** `Pit(3,1)=T, Wumpus(1,4)=tentative, OK(2,2)=T`
- **Decision:** Move to (2,2) — confirmed safe.

### Step 4 — at (2,2), Percept: NOTHING

- **Prop:** `¬S[2][2]=T, ¬B[2][2]=T`
- **MP via W1+W2:** all 4 neighbors safe → `OK(1,2), OK(3,2), OK(2,1), OK(2,3)` all T
- **Decision:** Path to gold runs (2,2)→(2,3)→(2,4). Need to confirm Wumpus(1,4) before approaching (2,4).

### Step 5 — SHOOT ARROW → (1,4)

- **Prop:** `Scream=T → WumpusDead=T`
- **MP via Rule W:** Arrow in direction of Wumpus + Scream heard ⇒ `Wumpus(1,4) eliminated`
- **FOL:** `∀x ∀y: Scream → ¬∃a∃b: LiveWumpus(a,b)` (after firing)
- **KB update:** `WumpusDead=T, OK(2,3)=T` [Wumpus dead, Breeze(2,3) means Pit adjacent (3,3)]
- **Decision:** Move (2,2)→(2,3)→(2,4).

### Step 6 — at (2,3), Percepts: BREEZE only

- **W2:** Breeze(2,3) ⇒ Pit adjacent. Options: (1,3), (3,3), (2,2), (2,4). Already know (1,3), (2,2), (2,4) safe. ⇒ **`Pit(3,3)=CONFIRMED`** ✓
- **Decision:** Move to (2,4) — already confirmed safe.

### Step 7 — at (2,4), Percepts: STENCH + BREEZE + GLITTER

- **W3:** `Glitter(2,4) ↔ Gold(2,4)` ⇒ **GOLD HERE — GRAB IT!** ⭐
- **MP:** `Glitter(2,4)=T + Rule W3 ⇒ Gold(2,4)=T`
- **FOL:** `∀x∀y: Glitter(x,y) ↔ Gold(x,y)` instantiated at (2,4) ⇒ `Gold(2,4)=T`
- **Stench at (2,4):** Wumpus already dead — ignore.
- **Breeze at (2,4):** Confirms Pit(3,4) (already known from map).
- **Action:** Grab gold. Return path: (2,4)→(2,3)→(2,2)→(1,2)→(1,1). Each step verified by `OK()` check.

---

## 6. Memory Table — 7 Steps in 7 Cells

| Step | Room | Percept | Key Conclusion | Move |
|---|---|---|---|---|
| 1 | (1,1) | Nothing | OK(1,2), OK(2,1) | → (2,1) |
| 2 | (2,1) | Breeze | Pit(3,1) ∨ Pit(2,2) — uncertain | ← back to (1,1) |
| 3 | (1,2) | Stench | **Pit(3,1) ✓**, Wumpus(1,4) tentative | → (2,2) |
| 4 | (2,2) | Nothing | All 4 neighbors safe | plan path |
| 5 | shoot↑ | Scream | **Wumpus(1,4) DEAD** | → (2,3) |
| 6 | (2,3) | Breeze | **Pit(3,3) ✓** | → (2,4) |
| 7 | (2,4) | Glitter | **GOLD GRABBED** ⭐ | return home |

---

## 7. Core Insight (5.5)

**At Step 3, the agent NEVER SAW Pit(3,1) or Wumpus(1,4). It DEDUCED both by combining observations across multiple rooms.**

The agent stays alive because it **REASONS before it acts**, not because it was lucky.

The 4 logical tools used:
1. Store percepts as T/F propositions in KB
2. Apply fixed world rules (W1–W5) as logical sentences
3. Use Modus Ponens to derive new facts
4. Use Resolution to eliminate impossible options

---

## 8. PL → FOL Upgrade (5.7)

**Why PL fails:** 4×4 grid = 16 separate breeze rules. 100×100 grid = 10,000 rules. Cannot generalize.

**FOL fix:** ONE universal rule covers any grid size.

```
∀x ∀y: Breeze(x,y) ↔ ∃a ∃b: Adjacent(x,y,a,b) ∧ Pit(a,b)
```

**Reading:** "For every room (x,y) — Breeze exists IFF some adjacent room (a,b) contains a Pit."

**Three upgrades FOL provides:**
- ∀ — speaks about all rooms at once
- ∃ — says SOME adjacent room has a pit (don't need to name each)
- Variables x,y,a,b — placeholders, generalize to any grid

---

## 9. FOL Inference — Proving Pit(3,1) End-to-End

**Step 1:** Start with universal rule W2:
`∀x ∀y: Breeze(x,y) ↔ ∃a ∃b: Adjacent(x,y,a,b) ∧ Pit(a,b)`

**Step 2:** Universal Instantiation, substitute x=2, y=1:
`Breeze(2,1) ↔ ∃a ∃b: Adjacent(2,1,a,b) ∧ Pit(a,b)`
Adjacents of (2,1): (1,1), (3,1), (2,2).

**Step 3:** Modus Ponens with observed fact:
`Breeze(2,1)=T ⇒ ∃a∃b: Adjacent(2,1,a,b) ∧ Pit(a,b)=T`
⇒ `Pit(1,1) ∨ Pit(3,1) ∨ Pit(2,2)`

**Step 4:** Resolution (eliminate options):
- ¬Pit(1,1) — agent came from there safely
- ¬Pit(2,2) — from Step 3 deduction at (1,2)
- **∴ Pit(3,1)=T ✓ CONFIRMED**

This is the **canonical FOL inference proof** — Universal Instantiation → Modus Ponens → Resolution.

---

## 10. Why Wumpus Ties the Whole Course Together (5.7)

| Course concept | How it appears in Wumpus |
|---|---|
| DIKW pyramid | Percept (D) → room info (I) → pit/wumpus locations (K) → safe path home (W) |
| Declarative knowledge | W1–W5 stored as facts in KB |
| Procedural knowledge | Strategy: sense → infer → act |
| Propositional logic | Each room state is T/F (B[2][1]=T, etc.) |
| Knowledge base | Built and updated as agent explores |
| **Modus Ponens** | At every step: rule + percept → conclusion |
| Entailment | "Does my KB entail OK(x,y)?" |
| Resolution | Step 3: eliminate Pit possibilities |
| FOL | One rule replaces 16 propositional ones |

---

## 11. Practice Q (likely quiz patterns)

### Q17: Agent at (1,1), detects Breeze. What now?

**Prop:** `B[1][1]=T → P(1,2) ∨ P(2,1)`
**MP:** Pit adjacent — uncertain which.
**Action:** DO NOT move to either. Retreat or shoot? No — agent has no info to shoot. **Stay put / explore other direction first.** Risk = death.

### Q18: At (2,2), Percept = [Nothing — S=F, B=F]

| Adjacent | Status | Justification |
|---|---|---|
| (1,2) | SAFE | ¬B(2,2) → ¬P(1,2); ¬S(2,2) → ¬W(1,2); already visited |
| (3,2) | SAFE | ¬B(2,2) → ¬P(3,2); ¬S(2,2) → ¬W(3,2) |
| (2,1) | SAFE | already visited |
| (2,3) | SAFE | ¬B(2,2) → ¬P(2,3); ¬S(2,2) → ¬W(2,3) |

All 4 neighbors confirmed SAFE.

### Q19: Why is this a "knowledge-based agent"?

Because the agent doesn't follow programmed instructions — it **reasons from sensed data + KB rules** to make new decisions. Different from fixed-script robot: agent must derive Pit/Wumpus locations through inference, never observed directly.

### Q20: Safe return path from (2,4) to (1,1)

`(2,4) → (2,3) → (2,2) → (1,2) → (1,1)` — every room verified by checking `OK()` against `¬Pit ∧ ¬Wumpus`. All 5 rooms confirmed safe by accumulated KB.

---

## 12. Quiz Self-Check (60 sec)

Cover answers, recite:

1. **5 world rules?** → W1 Stench↔Wumpus adj, W2 Breeze↔Pit adj, W3 Glitter↔Gold same room, W4 death, W5 safety
2. **5 percepts?** → Stench, Breeze, Glitter, Bump, Scream
3. **What does Stench mean?** → Wumpus adjacent (not here)
4. **What does Glitter mean?** → Gold in THIS room
5. **At (2,1) with Breeze, what's the conclusion?** → Pit(3,1) ∨ Pit(2,2) — uncertain
6. **How does agent confirm Pit(3,1)?** → At (1,2), no breeze ⇒ ¬Pit(2,2). By elimination, Pit(3,1)
7. **FOL form of W2?** → `∀x∀y: Breeze(x,y) ↔ ∃a∃b: Adjacent(x,y,a,b) ∧ Pit(a,b)`
8. **How many propositional rules vs FOL rules in 100×100 grid?** → 10,000 vs 1
9. **Three layers of logic used at every step?** → Propositional (KB symbols), Modus Ponens (chain), FOL (universal rule)

If all 9 in 60 sec → **Wumpus quiz-ready.**

---

## 13. Roman Urdu Hooks

| Concept | Hook |
|---|---|
| Stench | *"Wumpus paas hai, lekin yahaan nahi"* |
| Breeze | *"Pit paas hai, lekin yahaan nahi"* |
| Glitter | *"Gold yahin hai — uthao!"* |
| Step 3 reasoning | *"Pit aur Wumpus dekha nahi, deduce kiya"* |
| FOL upgrade | *"PL har room ke liye alag rule, FOL aik rule sab rooms pe"* |
| W2 FOL | *"Har room (x,y) — agar Breeze hai, koi adjacent room mein Pit zaroor hai"* |

---

## 14. Common Quiz Traps

1. **Stench means Wumpus HERE** ❌ — means adjacent
2. **Breeze means walk in = die** ❌ — means Pit adjacent, can still walk through breeze room
3. **Forgetting to check OK before moving** ❌ — every move requires `¬Pit ∧ ¬Wumpus`
4. **Mixing Glitter with Stench/Breeze adjacency** ❌ — Glitter is HERE, others are adjacent
5. **Using FOL `→` with `∃`** ❌ — same Mistake #1 from L1 Session 2 (∃ pairs with ∧)
6. **Listing 4 propositional rules in 4×4 grid for one concept** — actually 16 rules (one per room)
