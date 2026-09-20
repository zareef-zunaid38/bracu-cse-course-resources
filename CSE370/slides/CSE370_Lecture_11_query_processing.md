# CSE370 Lecture 11: Query Processing

BRAC University CSE370: Database Systems slide deck, 15 slides. Clean digital slides
(white background, plain text + diagrams), fully legible — transcribed with confidence.
Covers Topic 22 (Query Processing half) — the mechanics of turning an SQL query into an
executable plan and running it; Lecture 12 covers the Optimization half.

## Content

**Query processing steps (4-stage pipeline):**
1. **Query Compilation:** reading, validating, and translating the SQL query into a
   relational algebra statement (its "immediate form").
2. **Query Optimization:** modifying the relational algebra statement and choosing an
   execution strategy that reduces storage, memory, computation, and networking cost.
3. **Code Generation:** generating executable code (interpreted or run directly against
   the DB files) — essentially "glue" stitching together existing implementations of
   relational-algebra building blocks (selection, join, projection, aggregate, etc.).
4. **Query Execution:** the runtime processor actually runs the generated code and
   returns the result.
Pipeline diagram: Query (high-level) → Scanning/parsing/validating → immediate-form
query → Query optimizer → execution plan → Query code generator → executable code →
Runtime database processor → result.

**Query Compilation detail:** the query is compiled into relational algebra because RA
is a formal, recursively-constructed mathematical expression with formal transformation
rules that can rewrite it for performance without changing meaning. Example:
`SELECT MAX(Salary) FROM EMPLOYEE WHERE Dno=5` → `ℑ_MaxSalary(σ_Dno=5(EMPLOYEE))`.

**Query Tree:** the processor represents the RA expression as a tree rather than working
with the expression directly, because the tree makes the operation order and how
intermediate results feed later operations visually explicit — this makes it easy to
decide how to stitch together building-block code for the whole executable. Leaves of
the tree are always the original relations. Most query-optimization transformations are
expressed as modifying/shuffling branches of this tree. Worked 3-join example shown:
`π(...) (((σ_Plocation='Stafford'(PROJECT)) ⋈ Dnum=Dnumber (DEPARTMENT)) ⋈
Mgr_ssn=Ssn (EMPLOYEE))` drawn as a tree with PROJECT/DEPARTMENT/EMPLOYEE as leaves and
selection→join→join→projection as the bottom-up operation order.

**Key algorithmic components:** the query processor's building blocks are alternative
algorithms for: external sorting, selection, join, projection, set operations, aggregate
operations. Whether a given algorithm applies to a particular query depends on: (1)
existence/nature of indexes on relevant attributes, (2) volume and statistical
distribution of records in the underlying relations, (3) how well that algorithm fits
into the overall combination plan the optimizer is building.

**Query pipelining:** building-block operations act as streams/iterators returning one
tuple at a time. A child operation's returned tuple is immediately consumed by its
parent, which processes it and either passes it further up or discards it and pulls the
next tuple from the child — so tuples move up one at a time from leaves to root, being
modified as they move. This drastically cuts buffering requirements and improves both
single-query performance and concurrent-query throughput. **Limitation:** a join breaks
pipelining — at least one branch of the join must be fully buffered ("materialized") to
match against the incoming stream from the other branch. If data volume is too high,
materialization forces intermediate results onto disk. Worked example (hash-join over
merge-join over two index/linear-scan selections): shows which branch gets materialized
vs. pipelined, noting that choosing which branch to materialize is itself an
optimization question.

**External sorting:** applies whenever records must be sorted by an attribute the file
isn't already physically ordered by (typically to make a later join easier). Merge sort
is the near-universal choice. Implementation departs from the classic recursive
CS-textbook version in two ways: (1) partition size for the sort phase is chosen based on
disk block size / RAM page size (the unit of a single disk read), not by recursively
halving down to pairs; (2) the merge phase often merges **several sorted partitions
simultaneously**, not strictly two at a time — reducing the number of I/O passes needed.
Diagram: sort phase creates 1-page runs, then merge passes double the run size each pass
(1→2→4→8 pages) until one fully sorted output file remains. Multi-way merge diagram:
K sorted chunks, one input buffer per chunk plus one output buffer, repeatedly moving
the record with the smallest sort key to the output.

**General strategies for selection (single attribute):**
- **Table scan:** brute-force (linear scan start to end) or binary-search scan (only
  possible if records are physically sorted by the searched attribute).
- **Index-based search:** available when an index exists on the searched attribute —
  implementation differs by index type (primary, clustering, secondary B-tree, bitmap).
  Index-based search is generally preferable since it reads far less data from files.
- **Range conditions (<, <=, >, >=) on a tree-based index:** the index's own sorted
  structure is exploited via **index seek** (jump directly to the first leaf satisfying
  the condition) followed by **index scan** (sequentially read consecutive leaves while
  the range condition still holds).

**Conjunctive vs. disjunctive selections:**
- **Disjunction (OR):** implemented as a pipeline — each record is checked against each
  condition individually; if any condition is true, the record is retained and passed up;
  otherwise discarded.
- **Conjunction (AND):** three alternatives exist — (1) single-index search (use an
  index on one condition's attribute, then check the other conditions on the retrieved
  records); (2) composite index (an index over all the selection's attributes — select
  directly on it); (3) intersecting record pointers (do separate index searches, keep
  pointer sets, intersect them, then fetch the intersection's records). **In practice,
  DBMSs typically implement conjunctive selection as a chain of single-attribute
  selections instead**, since this gives the optimizer far more opportunities to
  reorder/push down individual selection blocks in the query tree.

**Strategies for joins:** joins are the most performance-critical operation, since
combining relations can massively blow up intermediate data volume; joining always
requires breaking pipelining/materialization on at least one branch. Cartesian products
are rare in practice — almost always converted to a natural/inner/outer join instead.
Three implementation strategies:
- **Nested-loop join:** for each tuple on one branch, scan all tuples on the other branch
  for a match — a doubly-nested loop.
- **Merge join:** sort both branches separately, then merge the sorted streams based on
  the join attribute — materializes both sides.
- **Hash join:** hash-bucket one branch's tuples; as the other branch's tuples arrive in
  a pipelined stream, hash each and check the matching bucket, evaluating only that
  bucket's tuples to produce and stream up combined rows.

**Index-based join (special case):** possible only when the join attributes are indexed
in *both* participating relations. Instead of retrieving-then-joining via one of the
three algorithms above, the DBMS searches both index structures directly, computes the
**intersection of the indexing node values**, then produces combined tuples by following
the matching record pointers to the raw data. Note: for ordinary (non-index) joins,
indexing on relevant attributes is mostly used to speed up the *selection* steps before
the join, not the join algorithm choice itself.

**Projection strategy:** only one implementation strategy exists (unlike selection/join,
which have several). Projection operates iteratively — for every tuple retrieved from
the earlier stage of the query tree, unwanted attributes are stripped immediately before
the tuple moves forward. It never accumulates a batch of tuples in a buffer before
filtering, since that would add unnecessary buffering overhead.

**Aggregate function strategy — Split-Apply-Combine pipeline:** the single general
strategy for MIN/MAX/SUM/AVG/COUNT. Tuples are **split** into buckets by the grouping
attribute's distinct values, the aggregate function is **applied** within each bucket
independently, then the per-bucket results are **combined** into the final output.
Optimization: splitting is done in pipelined fashion — the DBMS does not accumulate
tuples in separate physical buckets; instead, when a new tuple arrives for a group, a
**running group function** (e.g. a running total/counter) inside an active tracking
buffer is updated immediately.

**Performance concerns (cost factors query processing must account for):**
- **Storage access:** number of disk reads for index/data-file record retrieval, plus
  read/write for holding intermediate results.
- **Buffer consumption:** peak memory used for buffering intermediate results during
  execution.
- **Computation time:** total time to run the whole query.
- **Communication overhead:** latency/bandwidth for internal messaging among DBMS
  components during execution.
These matter little at low data volume, but become critical at real-world scale, since
production DBMSs must serve hundreds-to-thousands of concurrent queries.

**Closing slide (transition into Lecture 12):** frames query optimization's high-level
technique tree — Additional structure (Index [static/dynamic], Materialized view
[static/dynamic materialization]), Partitioning & placement (data partitioning, query
partitioning), Data Storage & Processing (Map-Reduce, bit vector store, Caching
[static/dynamic]), and Query Plan Optimization (simple vs. multiple-queries
optimization) — setting up Lecture 12's deeper dive into the optimization side.
