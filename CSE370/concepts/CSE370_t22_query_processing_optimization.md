---
type: concept
course: CSE370
topic: "Query Processing & Optimization"
difficulty: 3
thesis_relevance: —
status: draft
date: 2026-09-06
prerequisites: ["CSE370_t19_indexing.md"]
---

# Query Processing & Optimization
**Course:** CSE370 — Database Systems
**Date:** 2026-09-06

**Scope note:** Covers Topic 22 (Query Processing and Optimization) from `cse370_topics.md`. Spans two lecture decks: `CSE370_Lecture_11_query_processing.pdf` (the processing pipeline, pipelining, algorithm choices per operation) and `CSE370_Lecture_12_query_optimization.pdf` (heuristic algebraic rules, cost-based optimization, database catalog/histograms, physical optimization). Confirmed not on Midterm; part of the working Final theory syllabus (Topics 19–24, confirmed 2026-09-06 — see `cse370_exam_notes.md`).

**Sources used:** `CSE370_Lecture_11_query_processing.pdf` and `CSE370_Lecture_12_query_optimization.pdf` (both read via their sidecars, per the vault's sidecar rule), `CSE370_practice_ch12_query_processing.md` (14 Q&A, already a sidecar) and `CSE370_practice_ch13_query_optimization.pdf` (theory + algebraic + cost-based + critical-reasoning questions, with solutions — real text layer, no sidecar needed; **updated 2026-09-13** — Algebraic/Cost-based/Critical-Reasoning Q1–2 cited below unchanged, additions are a new Theory Questions section and a third algebraic-optimization worked example, Hospital DB Patient/PatientTreatment/Treatment, same 5-step procedure as the Aquarius example below); `CSE370_Lecture_12_heuristic_optimization_supplement.pdf` (**added 2026-09-13** — an 11-slide condensed handout NFF confirmed using directly in class for this material, clean text layer, no sidecar needed; cross-checking it against this note's Heuristic Rules table surfaced a missing rule, now added — see table below). No PYQs exist for CSE370 (confirmed).

> [!note] Textbook sourcing corrected 2026-09-06 (audit): `cse370_resources.md` maps this topic to Silberschatz 6e Ch 12–13, but the HDD copy of that PDF is a 94-page front-matter excerpt that ends at Chapter 3 — Ch 12–13 are not actually present. That citation has been removed rather than left implying the chapters were read. This note is built entirely from the lecture slides and practice sheets above.

---

## Definition

**Query processing** is the sequence of activities a DBMS undertakes to retrieve the results corresponding to an SQL query. **Query optimization** is the step within that process where the DBMS converts the query into an equivalent form expected to perform better, and chooses the best execution strategy among many alternatives — because SQL is *declarative* (states what to retrieve, not how), the DBMS must decide the how itself.

**Important framing:** "optimization" is a slightly misleading name — the resulting plan is not guaranteed to be the mathematically optimal one, only improved to the best of the system's practical ability within a reasonable search effort.

---

## Key Properties

### The Query Processing Pipeline (4 steps)

| Step | What happens |
|---|---|
| **1. Query Compilation** | Reading, validating, and translating the SQL query into a relational algebra expression |
| **2. Query Optimization** | Modifying the RA expression and choosing an execution strategy to reduce storage/memory/computation/network cost |
| **3. Code Generation** | Generating executable code that glues together implementations of RA building blocks (selection, join, projection, aggregate, etc.) |
| **4. Query Execution** | The runtime processor actually runs the generated code and returns the result |

**Query Tree:** the RA expression is represented as a tree rather than worked with directly, because the tree makes the operation order and how intermediate results feed later operations visually explicit — leaves are always the original relations. Most optimization transformations are expressed as modifying/shuffling branches of this tree.

**Which algorithm applies to a given operation** depends on: (1) existence/nature of indexes on relevant attributes, (2) volume and statistical distribution of records in the underlying relations, (3) how well that algorithm fits the overall combination plan being built.

### Query Pipelining

Building-block operations act as streams/iterators returning **one tuple at a time**: a child's returned tuple is immediately consumed by its parent, which either passes it further up or discards it and pulls the next tuple. This drastically cuts buffering requirements and improves both single-query performance and concurrent throughput.

**Critical limitation — joins break pipelining:** at least one branch of a join must be fully buffered (**materialized**) to be matched against the incoming stream from the other branch. If data volume is too high, materialization forces intermediate results onto disk. Deciding *which* branch to materialize (or whether both should be) is itself an optimization question.

### External Sorting

Applies whenever records must be sorted by an attribute the file isn't already physically ordered by (often to make a later join easier). Merge sort is the near-universal choice, but the real implementation departs from the classic recursive CS-textbook version in two ways:
1. **Partition size** is chosen based on disk block size / RAM page size (the unit of a single disk read) — not by recursively halving down to pairs.
2. The merge phase often merges **several sorted partitions simultaneously**, not strictly two at a time — reducing the number of I/O passes.

### Algorithm Strategies Per Operation

| Operation | Strategies |
|---|---|
| **Selection (single attribute)** | **Table scan** — brute-force (linear) or binary-search (only if records are physically sorted by that attribute). **Index-based search** — generally preferable, reads far less data; implementation differs by index type (primary, clustering, secondary B-tree, bitmap). |
| **Selection (range: <, <=, >, >=) with a tree index** | **Index seek** (jump to first leaf satisfying the condition) then **index scan** (read consecutive leaves while the condition still holds) |
| **Selection (disjunction, OR)** | Pipeline: check each record against each condition individually; retain if any is true |
| **Selection (conjunction, AND)** | Single-index search (index one attribute, check rest on retrieved rows); composite index (index over all attributes, select directly); intersecting record pointers (separate searches, intersect pointer sets). **In practice, DBMSs implement conjunctive selection as a chain of single-attribute selections** — gives the optimizer more reordering/push-down opportunities. |
| **Join** | **Nested-loop** (doubly-nested scan — for each tuple on one side, scan all of the other); **Merge join** (sort both sides, merge on join attribute — materializes both); **Hash join** (hash-bucket one side; pipeline the other side's tuples, checking matching buckets) |
| **Index-based join** (special case) | Only when the join attributes are indexed in **both** relations — search both index structures, compute the **intersection of index node values**, then fetch combined tuples via the matching record pointers |
| **Projection** | Only **one** strategy exists — iterative: for every tuple from the earlier stage, strip unwanted attributes immediately, never accumulating a batch first (would add buffering overhead) |
| **Aggregate functions** | **Split-Apply-Combine**: split tuples into buckets by the grouping attribute, apply the function per bucket, combine per-bucket results. Optimization: splitting is pipelined — a **running group function** (running total/counter) updates immediately per arriving tuple, no physical bucket accumulation |

**Note on indexing and joins:** for the three general join algorithms (nested-loop/merge/hash), index existence mostly matters for speeding up the *selection* steps that happen before the join, not the join algorithm choice itself — except in the index-based join special case above.

### Cost Factors in Query Processing

| Factor | What it measures |
|---|---|
| **Storage access** | Disk reads for index/data-file retrieval, plus read/write for intermediate results |
| **Buffer consumption** | Peak memory used for buffering intermediate results |
| **Computation time** | Total time to run the whole query |
| **Communication overhead** | Latency/bandwidth for internal DBMS component messaging |

These matter little at low data volume but become critical at real-world scale, since production DBMSs serve hundreds-to-thousands of concurrent queries.

### Query Optimization — Four Components

1. **Heuristic Rule-Based Algebraic Optimizations** — rewrite the query tree using formal RA transformation rules that preserve meaning while (expected to) reducing cost. Query optimization itself is costly, so extensive optimization is reserved for frequently-used, compiled queries; sporadic/interpreted queries often skip steps — DBMSs also **cache optimized execution plans** for important queries.
2. **Cost-Based Optimizations** — see below.
3. **Query Implementation Strategy Selection** — choosing among the algorithm alternatives per operation (see table above).
4. **Physical Optimization** — bottom-up vs. top-down execution, sharing intermediate results across queries, parallel execution via threads/processes, query plan caching.

### Heuristic Rules for Algebraic Modification (nine-rule set, confirmed 2026-09-13 against NFF's own class-used handout `CSE370_Lecture_12_heuristic_optimization_supplement.pdf` — the Lecture 12 sidecar additionally lists two further niche set-difference/intersection variants, kept out of this table as redundant with the set-ops row below)

| Rule | Statement |
|---|---|
| Cascade of σ | `σ_c1 AND c2...cn(R) ≡ σ_c1(σ_c2(...σ_cn(R)...))` |
| Commutativity of σ | `σ_c1(σ_c2(R)) ≡ σ_c2(σ_c1(R))` |
| Cascade of π | `π_List1(π_List2(...(R)...)) ≡ π_List1(R)` — all but the last are redundant |
| Commuting σ with π | If the selection condition only uses attributes already in the projection list, the two can swap |
| Commutativity of ⋈ (and ×) | `R ⋈_c S ≡ S ⋈_c R` |
| Commuting σ with ⋈ | If condition c only involves one joined relation R's attributes: `σ_c(R ⋈ S) ≡ (σ_c(R)) ⋈ S` |
| Commuting σ with set ops (∪,∩,−) | `σ_c(R θ S) ≡ (σ_c(R)) θ (σ_c(S))` |
| Converting (σ,×) into ⋈ | If σ after × is exactly a join condition: `σ_c(R × S) ≡ R ⋈_c S` |
| **π commutes with ∪** | `π_L(R ∪ S) ≡ (π_L(R)) ∪ (π_L(S))` — project each side first, then union the (smaller) results |

**Practical application procedure:**
1. Decompose composite select/project operations using cascading rules.
2. Move select/project operations downward, as close to the leaves as possible, via commutative rules.
3. Order selects so the **most restrictive** select runs first.
4. Convert any Cartesian product into a join using available selection clauses.
5. Identify sub-trees a single algorithm can execute together.

**Caveat:** some modifications may obstruct others — the optimizer must choose which to apply based on cost-effectiveness, not apply every rule blindly.

### Cost-Based Optimization

Heuristics alone can't capture everything — they ignore actual storage structures, index existence, and record distribution. Cost-based optimization fills this gap:

- **Database catalog:** metadata about data, one entry per relation typically — file size, record count, average record size, block count, blocking factor, index blocks/structure, number of distinct values per attribute, attribute selectivity, min/max values, etc.
- **Histograms:** without one, the system must assume a **uniform distribution** of values — too simplistic to be useful. A frequency histogram charts the actual distribution, letting the optimizer estimate a condition's true **selectivity** accurately. Both catalog and histograms must be kept up to date on a schedule for estimates to remain accurate.
- **Cost function:** a weighted sum of disk access, buffer consumption, computation complexity, intermediate-result storage overhead, and communication cost. Candidate query trees (from algebraic modification) are evaluated against it using catalog + histogram data; the estimated-best candidate is selected.
- **Search space is exponential** — the cost-based optimizer uses dynamic programming and heuristic-based pruning to explore only a reasonable subset, not the full space.

---

## Worked Example

**Full algebraic optimization walkthrough** (from Lecture 12): query `Select Lname from Employee, Project, Works_On where Pname='Aquarius' and Pnumber=Pno and Essn=Ssn and Bdate>'1957-12-31'`. The slides show this as a 4-transformation sequence, tree (a) through (e):

1. **(a) Naive tree:** one big conjunctive σ sitting above a chain of Cartesian products of all three relations.
2. **(a)→(b) Cascade + push down:** split the conjunctive σ into its four individual conditions and push each down to sit directly above only the relation(s) it needs: `σ_Bdate>'1957-12-31'` goes immediately above EMPLOYEE; `σ_Essn=Ssn` sits above the (Bdate-filtered EMPLOYEE)×WORKS_ON pairing; `σ_Pname='Aquarius'` goes immediately above PROJECT; `σ_Pnumber=Pno` stays at the top, since it's the condition joining the PROJECT branch to the EMPLOYEE/WORKS_ON branch.
3. **(b)→(c) Reorder (reassociate) the joins:** the tree is restructured so PROJECT×WORKS_ON (matched via `σ_Pnumber=Pno`) is computed as the *inner* sub-join first, with `σ_Essn=Ssn` promoted to the top join against the Bdate-filtered EMPLOYEE — a cheaper intermediate result than the original grouping in (b), since the Pname-filtered PROJECT⋈WORKS_ON pairing is smaller than joining EMPLOYEE with WORKS_ON first.
4. **(c)→(d) Convert (σ,×) sequences into proper ⋈:** each remaining selection-over-Cartesian-product pair is rewritten as an actual join operator (`⋈_Pnumber=Pno`, `⋈_Essn=Ssn`), matching the "converting a (σ,×) sequence into ⋈" heuristic rule above.
5. **(d)→(e) Push projections too:** projections (`π_Pnumber`, `π_Essn,Pno`, `π_Essn`, `π_Ssn,Lname`) are pushed down alongside the selections, so only the columns actually needed by later joins are carried through — not full rows.

**Equivalent final SQL** (mirrors tree (c)/(d)'s structure exactly): filter Employee by Bdate first (`e`), filter Project by Pname and join with Works_On on Pnumber=Pno (`pw`), then join `e` with `pw` on Essn=Ssn.

**Take-away pattern for exam questions of this type:** (a) break a conjunctive WHERE into individual conditions and push each down to sit directly above the *single* relation it needs; (b) reorder/reassociate joins so the cheapest (most-filtered, smallest) sub-join happens first; (c) convert Cartesian-product-plus-matching-selection pairs into actual joins; (d) push projections down too, once selections and join order are settled. Steps (a) and (b) are easy to conflate — a selection sitting "above one relation" (step a) is not the same transformation as *which join happens first* (step b); an exam answer that only does step (a) has not finished the optimization.

**Join-order combinatorics** (from Lecture 12, illustrating why cost-based search needs pruning): for a 4-relation join — ordering alone gives 4C2 × 3C2-equivalent = **18 orderings**; × 4 implementation strategies (sort-merge, nested-loop, partitioned hash, index-based nested-loop) = **72 alternatives**; × pipelined-or-materialized choice per join = **144 alternatives** — before even factoring in select/project/group-by choices.

**Cost-based join-order reasoning** (from `CSE370_practice_ch13_query_optimization.pdf`, Q1–2 of the Cost-based section): given a catalog showing DEPARTMENT has far fewer records than EMPLOYEE or PROJECT, join DEPARTMENT's records first — smaller relations joined earlier keep the intermediate result set smaller. When a histogram shows very few EMPLOYEE records satisfy `salary > 80000` (a highly selective condition) versus an assumed-uniform ~1000 PROJECT records for `Pnumber > 1000`, push the more selective condition (on EMPLOYEE) down and retrieve those records first, before the join.

**Algebraic optimization practice, simple case** (`CSE370_practice_ch13_query_optimization.pdf`, Algebraic Optimization Q1): `Select Fname, Lname, Ssn from EMPLOYEE where address='Merul Badda' and Salary>50000`. Applying "push selection down, project early" gives the solution's relational algebra: `E1 = π_Fname,Lname,Ssn,Address,Salary(Employee)` (project down to only the needed columns first), `E2 = σ_salary>50000(σ_address='Merul Badda'(E1))` (cascade the conjunctive selection), `E3 = π_Fname,Lname,Ssn(E2)` (final projection to just the SELECT list). Pattern: even a single-table query benefits from an early projection to a *superset* of needed columns before filtering, then a final projection down to exactly what's asked for.

**Algebraic optimization practice, multi-join case** (ch13, Algebraic Optimization Q2): a 3-way join (Department ⋈ Project ⋈ Employee) with one selection condition per relation (`Project.Location`, `Employee.Address`, `Department.Mgr_start_date`). The solution's pattern: build one filtered-and-projected sub-expression *per relation* first (e.g. `E1 = σ_Address='Gulshan 1, Dhaka'(π_Fname,Lname,Ssn,Address(Employee))`), then join those pre-filtered sub-expressions together, then apply the final top-level projection — i.e., every selection and projection is resolved at the leaf level *before* any join happens, generalizing the single-table Q1 pattern to multi-way joins.

**Real-world composite-index + selection-pushing case** (ch13, Other Critical Reasoning Q1): an `Orders` table (50M rows) queried by `WHERE user_id = ? ORDER BY order_date DESC` is doing a full table scan. Two-part fix: (1) a **composite index on `(user_id, order_date)`** — an index-based search isolates one user's rows (cutting disk I/O from 50M to a handful) and the index's own order satisfies `ORDER BY` without a separate sort; (2) ensure the engine filters by `user_id` via the index *before* fetching full row data — the general "apply filtering as early as possible" principle applied to index design itself, not just query-tree shape.

**Real-world join-order + selection-pushing case** (ch13, Other Critical Reasoning Q2): joining Sales (10M rows), Products (5K), Regions (50) with filters `Region='North America'` and `Product_Category='Electronics'`. A cost-based optimizer joins the **smallest filtered table first** — Regions (50 rows) with the North-America-filtered Sales subset, then Products — since starting from the smallest intermediate result keeps every subsequent join cheap. Selection pushing here means applying both filters at the leaf level, before any join, so the bulk of the 10M-row Sales table never has to be joined against Products/Regions unfiltered.

---

## Connections
- **Prerequisites:** Indexing Concepts / B+ Tree Index (Topics 20–21) — `CSE370_t19_indexing.md`. Index existence and type directly determine which selection/join strategies are available.
- **Successors:** none within the CSE370 topic map (Topic 22 is the last topic before Transactions/Concurrency Control, which are prerequisite-independent).
- **Research relevance:** — (core DBMS infrastructure; no direct connection to the thesis's AI-agents/knowledge-systems research direction).

---

## Common Mistakes

- Assuming query optimization guarantees the *mathematically optimal* plan — it only guarantees an improved plan found within a practical search effort (heuristic pruning, not exhaustive search).
- Forgetting that projection has only **one** implementation strategy (always iterative, strip-as-you-go) — unlike selection and join, which each have multiple named alternatives.
- Assuming index existence always speeds up a join directly — for the three general join algorithms, indexes are mainly used for the selection steps *before* the join; only the index-based join special case (both join attributes indexed) uses indexes for the join itself.
- Applying every heuristic rule blindly instead of recognizing that some modifications can obstruct others — the outline procedure (cascade → push down → order most-restrictive-first → convert to joins → group sub-trees) is a sequence, not an independent checklist.
- Treating "push selection down" as unconditionally correct without checking *which single relation* the condition's attributes actually belong to — a condition can only be pushed past a join if it needs attributes from just one side.
- Assuming uniform distribution is always a safe simplification for cost estimates — histograms exist specifically because real attribute distributions are usually skewed, and uniform-distribution assumptions can pick the wrong join order (see the worked salary-histogram example).

---

## Anki Cards

START
Basic
Why does a join operation break query pipelining, and what is the resulting technique called?
Back: At least one branch of the join must be fully buffered to be matched against the incoming stream from the other branch — this buffering is called materialization. If data volume is too high, materialization forces intermediate results onto disk.
END

START
Basic
In practice, how do DBMSs typically implement a conjunctive (AND) selection, and why?
Back: As a chain of single-attribute selections, even though single-index search, composite index, and intersecting-record-pointers are all theoretically possible. This gives the optimizer far more opportunities to reorder or push down individual selection blocks in the query tree.
END

START
Basic
What is the one general strategy used for implementing Projection, and why is it the only one?
Back: Iterative, strip-as-you-go: for every tuple retrieved from the earlier stage, unwanted attributes are stripped immediately before it moves forward — never accumulated in a buffer first, since that would add unnecessary buffering overhead. Unlike selection/join, there is no alternative strategy.
END

START
Basic
What is the Split-Apply-Combine pipeline used for, and how is the "split" stage optimized?
Back: It implements all aggregate functions (MIN/MAX/SUM/AVG/COUNT): tuples are split into buckets by the grouping attribute, the function is applied per bucket, results are combined. The split stage is pipelined — a running group function updates immediately as each tuple arrives, rather than physically accumulating tuples in separate buckets first.
END

START
Basic
Why does a DBMS maintain histograms in addition to the database catalog?
Back: Without a histogram, the optimizer must assume a uniform distribution of attribute values — often wrong and too simplistic. A frequency histogram charts the actual value distribution, letting the optimizer estimate a condition's true selectivity accurately, which can change which join order or selection-pushdown strategy is actually best.
END

START
Basic
When can an index-based join be used directly, bypassing the standard nested-loop/merge/hash join algorithms?
Back: Only when the join attributes are indexed in both participating relations. The DBMS searches both index structures, computes the intersection of the indexing node values, then produces combined tuples via the matching record pointers.
END

START
Basic
Why is the term "query optimization" considered slightly misleading?
Back: Because the resulting query is not necessarily made truly (mathematically) optimal — it is only improved to the best of the database system's practical ability, within a bounded/heuristically-pruned search, since the full space of alternative query trees can be exponential.
END

START
Basic
In the worked Aquarius/Employee/Project/Works_On example, what is the difference between the (a)→(b) transformation and the (b)→(c) transformation?
Back: (a)→(b) cascades the conjunctive selection and pushes each condition down to sit above only the single relation it needs. (b)→(c) is a separate transformation — reassociating (reordering) which join happens first, so the smaller/more-filtered sub-join (Project⋈Works_On) is computed before joining with Employee. Pushing a selection down and reordering joins are two distinct optimization moves, easy to conflate.
END

START
Basic
For the query `WHERE user_id = ? ORDER BY order_date DESC` on a 50-million-row Orders table doing a full scan, what index fixes both the filter and the sort?
Back: A composite index on (user_id, order_date). It lets the DBMS isolate one user's rows via index-based search (cutting I/O from 50M rows to a handful) and its own stored order already satisfies ORDER BY, avoiding a separate sort step.
END
