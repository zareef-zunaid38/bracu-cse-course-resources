---
course: CSE370
type: model-question-set
status: in-progress
assessment: Final
scope-confirmed: 2026-09-13
---

# CSE370 — Final Exam: Model Question Set

**Why this exists:** no PYQs exist for this course (confirmed 2026-09-05, `pyq/pyq_index.md`), so this set is built from scratch rather than adapted from real past papers. To stay honest about difficulty and style, every question here is either (a) an isomorphic relabeling of an already-verified worked example, (b) adapted from a solved practice-sheet problem, or (c) freshly built and fully solved by Claude before being posed — nothing is invented and shown unverified.

**Style anchor:** since no Final PYQs exist, style is modeled on the confirmed Midterm pattern (5 mandatory questions, each mixing theory + diagram/computation) plus the two already-sat quizzes covering this exact syllabus — Quiz 3 (B+ tree insertion + search trace) and Quiz 4 (concurrency scenario, 5 sub-questions). Valid anchor because Midterm and Final share the same creator (Central/BRACU); Quiz 3/4 share the same topic material even though a different creator (NFF).

**Confirmed scope** (`cse370_exam_notes.md`, official announcement 2026-09-13): Lectures 8–12 → Topics 19–24.
- Lecture 8 (Indexing) — all slides
- Lecture 9 (Transactions) — **pages 1–28 only** (MV2PL, pp.29–30, excluded)
- Lecture 10 (Concurrency Control) — all slides
- Lecture 11 (Query Processing) — all slides
- Lecture 12 (Query Optimization) — all slides

**How this works:** each of the 5 slots below (one per lecture) holds a cluster of alternative, same-weight, same-time-budget questions — only one would appear on the real exam, but we solve every variant in the cluster, so this single "paper" covers far more ground than a single guess would. Each variant is solved fully and privately by Claude before being posed in chat; the verified answer is recorded here once you've attempted it.

---

## Slot 1 — Lecture 8: Storage / Indexing / B+ Trees (Topics 19–21)

### (a) B+ Tree Insertion

Order **n = 4** (max 3 keys/leaf, max 3 keys/internal node). Insert into an empty tree, in order:

```
30, 70, 50, 80, 10, 40, 20, 120, 90, 60, 100, 110
```

Show the tree's state after every insertion that changes its shape. For each split: state whether it's a leaf or internal split, what key is pushed/copied to the parent and why that differs, and whether tree height increases.

**Difficulty:** Medium
**Status:** posed 2026-09-14, attempted 2026-09-16, verified correct.
**Answer:** Structural splits occur at insertions 80, 40, 90, and 110 (all others fit into an existing leaf with no shape change).
- **80:** leaf split (copy) — `[30,50,70,80]` → `[30,50]|[70,80]`, 70 copied to new root.
- **40:** leaf split (copy) — `[10,30,40,50]` → `[10,30]|[40,50]`, 40 copied up. Root `[40,70]`.
- **90:** leaf split (copy) — `[70,80,90,120]` → `[70,80]|[90,120]`, 90 copied up. Root `[40,70,90]`.
- **110:** leaf split (copy) — `[90,100,110,120]` → `[90,100]|[110,120]`, 110 copied up → root overflows to temp `[40,70,90,110]` (5 pointers) → **internal/root split (move)**: this course's convention (per `CSE370_t19_indexing.md` step-8 worked example) gives the left node ⌈5/2⌉ = 3 pointers/2 keys, right node the remaining 2 pointers/1 key — left `[40,70]`, right `[110]`, middle key **90 moved up** (not copied) as the new root. **Height increases to 3 levels** (root → internal → leaf).

Final tree:
```
Root:            [90]
Level-2 (int.):  [40, 70]                    [110]
Leaves:   [10,20,30]|[40,50,60]|[70,80]   [90,100]|[110,120]
```

### (b) B+ Tree Deletion

*Grounded in the 5-case deletion table in `CSE370_t19_indexing.md` and Assignment 3's own B+ tree deletion coverage; tree and sequence below are a fresh derivation, fully verified case-by-case before posing (not the practice sheet's exact 14-step sequence, since that PDF wasn't directly re-read for this draft).*

Order **n = 4** (max 3 keys/leaf, max 3 keys/internal node; min keys/leaf and min keys/internal = 1). Given the following B+ tree:

```
Root:            [200, 400]
Level-2 (int.):  [100]         [250, 300]         [450]
Leaves:   [50,80]|[100,150,180]  [200,220]|[250,270]|[300,350,380]  [400,420,430]|[450,480]
```
(Reading left to right: subtree under 100 covers [50,80] and [100,150,180]; subtree under [250,300] covers [200,220], [250,270], [300,350,380]; subtree under 450 covers [400,420,430] and [450,480].)

Delete, in order: **480, 430, 450, 420**. Show the tree's state after every deletion that changes its shape. For each step: state which of the 5 deletion cases applies, and whenever a deleted key also exists as a copy in an internal node, state explicitly what value replaces that copy.

**Difficulty:** Hard
**Status:** posed 2026-09-16, attempted — all 4 steps conceptually correct (step 4's drawing was incomplete/unclear on first pass, confirmed correct on clarification: 400 is the new internal node, with [300,350,380] as its left child and [400] as its right child).
**Answer:**
- **Delete 480:** Case 1a — leaf `[450,480]` (3>min... 2 keys, > min 1), just remove → `[450]`.
- **Delete 430:** Case 1a — leaf `[400,420,430]` (> min), just remove → `[400,420]`.
- **Delete 450:** leaf `[450]` = min → Case 2a, borrow from left sibling `[400,420]`: highest key (420) moves up. Left → `[400]`, right → `[420]`, parent separator updates 450→420.
- **Delete 420:** leaf `[420]` = min, sibling `[400]` = min (Case 2 fails) → parent (the `[420]` internal node, 1 key = min) also fails Case 3 → **Case 4a**: leaf merge empties the parent to 0 keys/1 child (deficient) → propagate: deficient node borrows from left sibling `[250,300]` (has extra) — root separator 400 drops down into the deficient node, sibling's rightmost child (`[300,350,380]`) transfers over, sibling's rightmost key (300) moves up to root.

Final tree:
```
Root:            [200, 300]
Level-2 (int.):  [100]         [250]         [400]
Leaves:   [50,80]|[100,150,180]  [200,220]|[250,270]  [300,350,380]|[400]
```

### (c) Index Theory

*Grounded directly in `CSE370_t19_indexing.md`'s Key Properties tables (Primary vs. Secondary, Dense vs. Sparse, Multilevel Indexing) and the Midterm's confirmed "1–2 line explain" convention for theory sub-questions.*

**(c-i)** Define what makes an index "primary," and state the common misconception this definition corrects. [2 marks]
**(c-ii)** Why can a sparse index only ever be built on a primary index, never a secondary one? [2 marks]
**(c-iii)** A table has a secondary index candidate attribute that, by coincidence, the table also happens to be physically sorted by. Is the resulting index primary or secondary? Justify in one line. [2 marks]
**(c-iv)** Explain why a two-level index (sparse outer index over a dense inner index) reduces disk I/O compared to a single flat dense index. [2 marks]
**(c-v)** True or False, with justification: "Any dense index can be converted to a sparse index to save space, regardless of whether the table is sorted by its search key." [2 marks]

**Difficulty:** Easy
**Status:** posed 2026-09-16, answers given directly (time-pressure pass, not self-attempted).
**Answer:**
- **(c-i)** Primary index = index whose key the table is physically sorted by (not "index on the PK" — that's the common misconception; a table can be sorted by a non-PK attribute instead).
- **(c-ii)** Sparse indexes rely on scanning forward from the nearest indexed entry, which only works if the table is physically sorted by that key — i.e., only on a primary index.
- **(c-iii)** Primary — status depends solely on physical sort order, not on whether the attribute is "normally" a secondary candidate.
- **(c-iv)** The small in-memory sparse outer index narrows the search to one block, replacing a disk-bound scan of the large dense index with one in-memory lookup + one disk read.
- **(c-v)** False — sparse only works when the table is sorted by the search key; converting an unsorted-table's dense index to sparse breaks correctness.

### (d) Hashing

*Fresh derivation (bucket construction + uniformity comparison, matching the shape of `CSE370_practice_ch11_indexing_hashing.pdf` Q5–Q7), fully solved and checked before posing.*

A hash index uses **5 buckets** (numbered 0–4), each holding a maximum of **2 records** before overflow chaining is needed. Insert the following 10 search keys, in this order:

```
12, 25, 7, 33, 41, 18, 9, 26, 14, 22
```

**(d-i)** Using h1(k) = k mod 5, show the resulting bucket structure, including any overflow chain(s). [4 marks]
**(d-ii)** Using h2(k) = ⌊k / 10⌋ mod 5, show the resulting bucket structure, including any overflow chain(s). [4 marks]
**(d-iii)** Which hash function distributes these keys more uniformly? Justify using the number of overflow buckets each required. [2 marks]

**Difficulty:** Medium
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

---

## Slot 2 — Lecture 9: Transactions (pp. 1–28 only, MV2PL excluded)

### (a) ACID Identification

*Fresh scenarios, each structurally mirroring one of the four verified violation patterns in `CSE370_t23_transactions.md`'s Q13 drill (partial execution → Atomicity; committed-but-lost → Durability; invalid state reached → Consistency; concurrent interference → Isolation).*

**(a-i)** An airline booking system deducts the fare from a customer's card, but a network failure occurs before the seat is marked as reserved. No rollback occurs, and the payment is not reversed. Which ACID property is violated? [2 marks]
**(a-ii)** A university enrollment system shows a student's course registration as confirmed on screen, but a server restart moments later reveals the registration was never actually written to the database. Which property is violated? [2 marks]
**(a-iii)** A retail system lets a single discount code be applied to the same order twice (no check on prior use), producing a negative final order total that is accepted into the database with no complaint from either subsystem. Which property is violated? [2 marks]
**(a-iv)** Two clerks simultaneously read the same starting inventory count for an item, each independently compute their own deduction, and both write back — one clerk's update silently overwrites the other's. Which property is violated? [2 marks]

**Difficulty:** Medium
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

### (b) Log-Based Recovery — pure UNDO case

*Order-preserving relabeling of `CSE370_t23_transactions.md`'s verified Case 1 (pure UNDO) worked example — same structure, new transaction/item names and values.*

Given the log below, followed by a system crash:
```
<T5 start>
<T5, X, 80, 120>
<T5, Y, 200, 250>
<T6 start>
<T6, Z, 60, 90>
-- SYSTEM CRASH --
```
State which transaction(s) must be undone and which must be redone, and give the final values of X, Y, and Z after recovery. [5 marks]

**Difficulty:** Easy
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

### (c) Log-Based Recovery — mixed UNDO/REDO case

*Order-preserving relabeling of `CSE370_t23_transactions.md`'s verified Case 2 (mixed UNDO/REDO) worked example — same structure, new transaction/item names and values.*

Given the log below, followed by a system crash:
```
<Ta start>
<Ta, P, 10, 20>
<Ta commit>
<Tb start>
<Tb, Q, 300, 400>
<Tc start>
<Tc, R, 5, 15>
<Tc commit>
-- SYSTEM CRASH --
```
State which transaction(s) must be undone and which must be redone, and give the final values of P, Q, and R after recovery. [5 marks]

**Difficulty:** Medium
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

### (d) Transaction States / Failure Causes

*Grounded directly in `CSE370_t23_transactions.md`'s transaction-state lifecycle diagram and the 6 documented causes of transaction failure.*

**(d-i)** List the transaction states, in order, along the "success path" from Begin to Terminated. [2 marks]
**(d-ii)** Name any three of the six documented causes of transaction failure. [3 marks]
**(d-iii)** Can a transaction that has reached the Failed state still eventually succeed? Explain in one line. [2 marks]

**Difficulty:** Easy
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

---

## Slot 3 — Lecture 10: Concurrency Control

### (a) Concurrency-Problem Scenario

*Quiz 4-style 5-part scenario (name the problem → ACID property → prevention → isolation-level reasoning → 2PL check), using a fresh Lost Update scenario — a different problem type from Quiz 4's own Dirty Read/Phantom Read scenario, grounded in the same worked pattern in `CSE370_t24_concurrency_control.md`.*

A warehouse system: T1 reads the stock count for Item Z (= 200) and plans to subtract 50 (a sale). Concurrently, T2 reads the same stock count (= 200, T1 hasn't written yet) and plans to subtract 30 (a sale via a different channel). T1 writes 150 and commits. T2, having read the original 200 before T1's write, computes 200 − 30 = 170 and writes 170, overwriting T1's update. The correct final count should have been 200 − 50 − 30 = 120.

**(a-i)** Identify the concurrency problem in this scenario. [2 marks]
**(a-ii)** Name the ACID property violated. [2 marks]
**(a-iii)** How could this have been prevented? [2 marks]
**(a-iv)** Does Repeatable Read isolation, by itself, guarantee this problem cannot occur? Justify with reference to the three-phenomena table. [2 marks]
**(a-v)** If both transactions instead used Strict 2PL with X-locks held until commit, would this Lost Update still occur? Explain briefly. [2 marks]

**Difficulty:** Medium
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

### (b) 2PL Lock-Schedule Legality

*Fresh derivation, grounded in the Growing/Shrinking/Lock-Point/Upgrade-Downgrade rules in `CSE370_t24_concurrency_control.md` and the graph-reading style noted for `CSE370_practice_ps10_concurrency_control.pdf`.*

**(b-i)** Is the following lock sequence for a single transaction T1 legal under Two-Phase Locking? `Lock-S(A), Lock-S(B), Lock-X(C), Unlock(B), Lock-S(D), Unlock(A), Unlock(C), Unlock(D)`. If not, identify exactly which step violates 2PL and why. [3 marks]
**(b-ii)** Given the corrected, legal reordering `Lock-S(A), Lock-S(B), Lock-X(C), Lock-S(D), Unlock(B), Unlock(A), Unlock(C), Unlock(D)`, identify T1's lock point. [2 marks]
**(b-iii)** In this corrected schedule, could T1 legally upgrade its `Lock-S(A)` to `Lock-X(A)` immediately after acquiring `Lock-S(D)`, assuming no other transaction holds a lock on A? State the condition required for a legal upgrade and whether it's satisfied here. [3 marks]

**Difficulty:** Hard
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

### (c) Timestamp-Ordering Trace

*Fresh derivation applying the exact Timestamp-Ordering rules documented in `CSE370_t24_concurrency_control.md` (read_TS/write_TS conditions), fully traced and verified before posing.*

Item X starts with `read_TS(X) = 0`, `write_TS(X) = 0`. Three transactions run concurrently: T1 (timestamp 5), T2 (timestamp 10), T3 (timestamp 15). Operations occur in this order:
1. T2 reads X.
2. T1 writes X.
3. T3 writes X.
4. T2 reads X again.

For each operation, state whether it proceeds normally or forces a rollback, and give the updated `read_TS(X)` / `write_TS(X)` after each operation that proceeds. [8 marks]

**Difficulty:** Medium
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

### (d) Deadlock — Wait-Die / Wound-Wait

*Fresh derivation, direct rule application from the Wait-Die/Wound-Wait table in `CSE370_t24_concurrency_control.md`.*

T1 (timestamp 10), T2 (timestamp 20), T3 (timestamp 30) — lower timestamp = older. For each event below, state the outcome under **(I) Wait-Die** and **(II) Wound-Wait**:

**(d-i)** T2 requests a resource currently held by T1. [4 marks]
**(d-ii)** T1 requests a resource currently held by T3. [4 marks]

**Difficulty:** Medium
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

---

## Slot 4 — Lecture 11: Query Processing

### (a) Algorithm-Strategy Selection

*Grounded directly in the Algorithm Strategies Per Operation table in `CSE370_t22_query_processing_optimization.md`.*

**(a-i)** A query filters Employee by `salary > 80000`. Employee has no index on salary, and records are not physically sorted by salary. What selection strategy must be used, and what is its cost implication? [2 marks]
**(a-ii)** The same table instead has a B+ tree index on salary. What two-step process retrieves the qualifying records? [2 marks]
**(a-iii)** A query joins Employee and Department, where both `Employee.dept_id` and `Department.dept_id` are indexed. Which special join strategy becomes available, and how does it work? [3 marks]
**(a-iv)** A query selects rows matching `dept='Sales' OR salary>90000` (a disjunction across two different attributes). What strategy must be used, and why can't a single index alone resolve it efficiently in general? [3 marks]

**Difficulty:** Medium
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

### (b) Pipelining vs. Materialization

*Grounded directly in the Query Pipelining section of `CSE370_t22_query_processing_optimization.md`.*

**(b-i)** Explain what it means for query-plan operators to be "pipelined," and why this reduces buffering requirements. [3 marks]
**(b-ii)** Why does a join operation break pure pipelining? What must happen to at least one of its input branches? [3 marks]
**(b-iii)** Given the query tree `π(σ(R) ⋈ σ(S))`, where S is a very large relation but `σ(S)` is highly selective (very few rows pass), which branch of the join would you choose to materialize, and why? [4 marks]

**Difficulty:** Easy
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

---

## Slot 5 — Lecture 12: Query Optimization

### (a) Heuristic Algebraic Rewrite

*Fresh query, same 5-step shape as the Aquarius worked example in `CSE370_t22_query_processing_optimization.md`. Follows the official 5-step procedure confirmed same-day via Central Discord announcement 2026-09-16 (`cse370_exam_notes.md`, Final Pattern): (1) Cascade of Selection, (2) Pushdown to relevant leaf, (3) most restrictive selection first — as its own separate tree, (4) Cartesian Product → Theta Join, (5) inject Projections. Deliverable shows the required minimum of 3 query trees.*

Relations: `STUDENT(Sid, Sname, Dept, GPA)`, `COURSE(Cid, Cname, Instructor)`, `ENROLLMENT(Sid, Cid, Grade)`.

Query: `SELECT Sname FROM Course, Student, Enrollment WHERE Dept='CSE' AND Grade='A' AND Sid=Enrollment.Sid AND Cid=Course.Cid;`

**Catalog note:** 5% of Student rows have `Dept='CSE'`; 30% of Enrollment rows have `Grade='A'`.

Optimize this query tree via algebraic modifications, following the required 5 steps and producing at minimum:
- **Tree 1** (Steps 1+2 combined): cascade the conjunctive WHERE into individual selections and push each down to the single relation it needs.
- **Tree 2** (Step 3 alone): apply the most restrictive selection first — using the catalog note above, reassociate the join structure accordingly.
- **Tree 3** (Steps 4+5 combined): convert every remaining (σ, ×) pair into a proper theta join, and push projections down to only the needed columns. [10 marks]

**Difficulty:** Hard
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

### (b) Cost-Based Reasoning

*Grounded in the Cost-Based Optimization section and the verified Join-Order Combinatorics figures in `CSE370_t22_query_processing_optimization.md`.*

**(b-i)** A catalog shows: TableA = 200 rows, TableB = 50,000 rows, TableC = 10 rows. A query joins all three. Using the "smallest/most-filtered relation joined first" heuristic, what join order would a cost-based optimizer likely choose, and why? [3 marks]
**(b-ii)** A histogram shows only 2% of TableB's rows satisfy `status='urgent'`, while an assumed-uniform estimate (with no histogram, and 5 possible status values) would predict roughly 20%. Why does the histogram change the optimizer's decision, and what's the risk of relying on the uniform assumption instead? [3 marks]
**(b-iii)** The lecture's combinatorial method gives **18 distinct join orderings** for a 4-relation join, before considering implementation strategy (`CSE370_t22_query_processing_optimization.md`'s verified figures: 18 orderings → ×4 strategies = 72 → ×2 for pipelined-or-materialized = 144). If a newer DBMS instead supports **5** implementation strategies per join, following the exact same multiplication method, how many total alternative execution plans result? Show your work. [4 marks]

**Difficulty:** Medium
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

---

## After completion

Fold the finished set + verified answers into the Final revision guide (`CSE370/revision-guides/`, per `project_revision_guide_workflow`) as a "Mock Final" section.
