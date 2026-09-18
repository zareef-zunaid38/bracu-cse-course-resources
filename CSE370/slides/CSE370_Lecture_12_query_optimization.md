# CSE370 Lecture 12: Query Optimization — sidecar (read this, not the .pdf)

BRAC University CSE370: Database Systems slide deck, 16 slides. Clean digital slides
(mixed dark title cards and light/dark content slides with sticky-note callouts), fully
legible — transcribed with confidence. Covers Topic 22 (Query Optimization half) —
continues directly from Lecture 11 (Query Processing).

## Content

**Introduction / Motivation:** an SQL query, executed exactly as written, can be very
inefficient (slow, resource-heavy) — a serious concern since a DBMS must answer many
queries simultaneously and fast. Also, even in its original form, different parts of a
query can be processed multiple ways with very different costs, since SQL is
**declarative** (states *what* to retrieve, not *how*) — the DBMS must decide the best
execution strategy among many alternatives. **Callout (important framing):** "optimization"
is a slightly misleading term — the query is not necessarily made truly optimal, only
improved to the best of the database system's ability (a bounded, practical search, not
a guarantee of the mathematically best plan).
**Objective:** convert the original query into an equivalent query expected to perform
much better, and choose the best execution strategy for that modified query.

**General principles (heuristics for what makes a query efficient):**
- Reduce the amount of data retrieved from disk as much as possible.
- Apply filtering as early as possible, to avoid consuming memory unnecessarily and for
  too long.
- Order operations to reduce the amount of computation over data.
- Choose the most cost-effective strategy for executing individual query blocks.
- Reduce disk storage overhead for intermediate results.
- Pipeline processing of query blocks whenever possible.
- When network is involved, minimize communication steps and bandwidth consumption.

**Four components of query optimization:**
1. **Heuristic Rule-Based Algebraic Optimizations**
2. **Cost-based Optimizations**
3. **Query Implementation Strategy Selection**
4. **Physical Optimization**
**Callout:** query optimization itself is a costly maneuver for the DB system, so
extensive optimization is reserved for frequently-used, compiled queries; sporadic,
one-off interpreted queries often skip some optimization steps. DB systems also often
**cache optimized execution plans** for their most important/frequent queries.

**Query Tree in Relational Algebra:** the DB system first translates the input SQL into
its formal relational-algebra form; any RA expression can be represented as a tree (the
**query tree**), which makes visible how the component parts of a query connect —
improvement opportunities are attempted directly over this tree structure. Heuristic
rule-based algebraic modifications transform the original query tree into another that
gives the *same result* but is expected to have lower cost/better performance — this is
the first step of query optimization.

**Some heuristic rules for algebraic modifications** (the slides explicitly say "check
the reference textbook for the whole list" — this is a partial, representative set):
- **Cascade of σ:** a conjunctive selection can be broken into a cascade (sequence) of
  individual σ operations: `σ_c1 AND c2 AND...cn(R) ≡ σ_c1(σ_c2(...σ_cn(R)...))`.
- **Commutativity of σ:** `σ_c1(σ_c2(R)) ≡ σ_c2(σ_c1(R))`.
- **Cascade of π:** in a cascade of π operations, all but the last are redundant:
  `π_List1(π_List2(...π_Listn(R)...)) ≡ π_List1(R)`.
- **Commuting σ with π:** if the selection condition only involves attributes already in
  the projection list, the two operations can be swapped.
- **Commutativity of ⋈ (and ×):** the join (and Cartesian product) operation is
  commutative: `R ⋈_c S ≡ S ⋈_c R`. (Attribute order in the result may differ, but
  meaning is the same, since attribute order isn't significant in the relational model.)
- **Commuting σ with ⋈ (or ×):** if the selection condition only involves one of the two
  joined relations' attributes (say R), the σ can be pushed down into just that side:
  `σ_c(R ⋈ S) ≡ (σ_c(R)) ⋈ S`.
- **Commuting σ with set operations (∪, ∩, −):** `σ_c(R θ S) ≡ (σ_c(R)) θ (σ_c(S))` for
  any of the three set operations.
- **The π operation commutes with ∪:** `π_L(R ∪ S) ≡ (π_L(R)) ∪ (π_L(S))`.
- **Converting a (σ, ×) sequence into ⋈:** if a σ following a × corresponds exactly to a
  join condition, convert the pair into a join: `σ_c(R × S) ≡ R ⋈_c S`.
- **Pushing σ with set difference:** `σ_c(R − S) = σ_c(R) − σ_c(S)`; note σ may be
  applied to only one relation: `σ_c(R − S) = σ_c(R) − S`.
- **Pushing σ to only one argument in ∩:** if all attributes in condition c belong only
  to relation R, `σ_c(R ∩ S) = σ_c(R) ∩ S`.

**Outline of algebraic modification application (the practical procedure):**
1. Decompose composite select and project operations using the cascading rules.
2. Move select and project operations downward as close to the leaves as possible, using
   the commutative rules.
3. Order the selects so the most restrictive select is applied first.
4. Try to convert any Cartesian product into a join using available selection clauses.
5. Identify sub-trees representing a group of operations that a single algorithm can
   perform together.
**Callout:** some modifications may obstruct others, so the query optimization engine has
to choose which to apply and which not to, based on cost-effectiveness analysis.

**Worked example — full algebraic optimization walkthrough** (Select Lname from
Employee, Project, Works_On where Pname='Aquarius' and Pnumber=Pno and Essn=Ssn and
Bdate>'1957-12-31'):
- (a) Naive tree: one big conjunctive σ sitting above a chain of two Cartesian products
  of all three relations.
- (b) Cascade the σ into three individual conditions, and push each down toward the
  relation(s) it actually needs — `σ_Pname='Aquarius'` goes directly above PROJECT,
  `σ_Essn=Ssn` and `σ_Bdate>...` sit above the EMPLOYEE⋈WORKS_ON subtree.
- (c) Convert the two remaining (σ, ×) pairs into proper joins (⋈), and push
  `σ_Bdate>'1957-12-31'` further down to sit directly above EMPLOYEE alone (since that
  condition only needs EMPLOYEE's attributes) — this is the most-optimized single query
  tree using algebraic rules only.
- **Final query rewrite (equivalent, decomposed into nested subqueries mirroring the
  optimized tree):** filters Employee by Bdate first (`e`), filters Project by Pname and
  joins with Works_On (`pw`), then joins `e` with `pw` on Essn=Ssn — each filter pushed
  as close to its source table as algebraically possible.
- A further diagram (d)→(e) shows pushing projections down alongside the selections
  (e.g. `π_Essn`, `π_Ssn,Lname`) so that only the needed columns are carried through each
  intermediate join, not full rows.

**Cost-based optimization — why it's needed beyond heuristics:** algebraic (heuristic)
modifications typically improve performance significantly, but heuristics can't capture
everything, because they ignore the *actual* storage structures, index existence, and
record distribution for the specific relations involved. Cost-based optimization fills
this gap by estimating storage, memory, computation, and networking cost for each
candidate modified query and picking the best-estimated one; cost assessments also help
choose the best execution strategy for individual query blocks.

**Database catalog:** metadata about data, typically one catalog entry per relation.
Contains: file/relation size, number of records/tuples, average record size, number of
file blocks, blocking factor (records per block), index blocks/indexing structure,
number of distinct values for important attributes, attribute selectivity (fraction of
records satisfying an equality condition), min/max values for important attributes, etc.
Worked example tables shown: per-column distinct-value/range stats (e.g. EMPLOYEE.Ssn:
10,000 distinct values, 1–10,000 range), per-relation row/block counts (e.g. EMPLOYEE:
10,000 rows, 2,000 blocks), and per-index stats (uniqueness, B-tree level count, leaf
block count, distinct key count).

**Histograms:** without extra data, the system must assume a **uniform distribution** of
records against interesting attributes — too simplistic to be very useful. A **frequency
histogram** charts the actual distribution of an attribute's values, letting the
optimizer estimate a condition's true **selectivity** far more accurately. Single-
attribute histograms are most common, but histograms over attribute sets or functions of
attributes are also sometimes kept. Worked example: a salary histogram for EMPLOYEE
showing a skewed (non-uniform) distribution across salary bands — most employees cluster
in the 40k–70k band, tapering off toward 200k–500k.
**Callout:** both the database catalog and histograms must be kept up to date (on some
designated schedule) for cost estimates to stay accurate.

**Technique of cost-based optimization:** the DBMS defines a cost function as a weighted
sum of disk access, buffer consumption, computation complexity, storage overhead for
intermediate results, and communication cost. Candidate alternative query trees
(produced by algebraic modification) are evaluated against this cost function using
catalog + histogram data, and the estimated-best candidate is selected for execution.
**The search space of alternative query trees can be exponential** — so the cost-based
optimizer uses dynamic programming and heuristic-based pruning to explore only a
reasonable subset of alternatives, not the full space.

**Worked example — combinatorial explosion of join alternatives** (4-relation join):
- Ordering alone: first join picks 2 of 4 relations (4C2 = 6 ways), second join picks 2
  of the remaining 2 (3C2... resolves to 3 ways after accounting for symmetry), fixing
  the last join → **6 × 3 = 18 orderings**.
- Each join can use one of 4 implementation strategies (sort-merge, regular nested-loop,
  partitioned hash-join, index-based nested loop) → **18 × 4 = 72 alternatives**.
- Each join can additionally be pipelined or materialized → **72 × 2 = 144 alternatives**
  — and this is *before* factoring in the further choices for select/project/group-by
  operations, which is exactly why the search space is so large and needs pruning.

**Physical optimization:** the alternative ways the final (rule- and cost-optimized)
query tree can actually be *processed* at runtime. Options: bottom-up vs. top-down
execution of query-tree parts; sharing intermediate results across inter/intra-query
parts; parallel execution via threads/processes; query plan caching; etc.
**Callout:** query optimization is one of the most active research areas in database
systems, given its complexity and importance to real-world performance.

**Closing slide:** teases Lecture 13 (title not yet revealed in this deck).
