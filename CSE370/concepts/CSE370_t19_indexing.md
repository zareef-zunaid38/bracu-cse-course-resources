---
type: concept
course: CSE370
topic: "Database Indexing (Ordered Indexes & B+ Trees)"
difficulty: 3
status: draft
date: 2026-09-06
prerequisites: []
---

# Database Indexing (Ordered Indexes & B+ Trees)
**Course:** CSE370 — Database Systems
**Date:** 2026-08-13 (Topic 19 section added 2026-09-06)

**Scope note:** Covers Topics 19 (Storage and File Organization), 20 (Indexing Concepts), and 21 (B+ Tree Index) from `cse370_topics.md`. Topics 20–21 were taught as one unit in `CSE370_Lecture_08_indexing_hashing.pdf` and tested as one unit (Quiz 3, 2026-08-19). **Topic 19 added 2026-09-06** to close a gap flagged since this note's creation: no lecture slide deck covers it at all (`CSE370_Lecture_08...` opens directly on indexing, no storage-layer introduction), and the local copy of Silberschatz 6e is only a 94-page front-matter/select-chapters excerpt that does not include Ch 10 — so the Topic 19 section below is written from standard, well-established database-systems theory (heap files, sorted files, clustering) common to any DB textbook, not transcribed from a specific cited page. Flagging this explicitly since it breaks from the usual slide/textbook-sourced convention. A brief Hashing section is included because the same lecture and practice sheet bundle it in, even though it isn't a numbered topic in the topic map — slides call it a rare alternative to B+ trees, not the primary examinable structure.

**Sources used:** `CSE370_Lecture_08_indexing_hashing.pdf` (primary for Topics 20–21 — this is what's actually examinable per the exam-notes rule), `CSE370_practice_ch11_indexing_hashing.pdf` (practice, solutions included). No PYQs exist for CSE370 yet (`pyq/pyq_index.md` — confirmed empty).

> [!note] Textbook sourcing corrected 2026-09-06 (audit): `cse370_resources.md` maps this topic to Silberschatz 6e Ch 10–11 and Elmasri & Navathe 7e Ch 16–17, but the local copies are excerpts — Silberschatz's ends at Chapter 3, E&N's ends mid-Chapter 1. None of those four chapters are actually present in either file. Both citations have been removed rather than left implying the chapters were read (this affects not just Topic 19, as originally flagged, but Topics 20–21 too). This note is built entirely from the lecture slide and practice sheet above.

---

## Definition

**Indexing** is an auxiliary data structure that speeds up data access. Without an index, answering a query like "find the instructor with ID 76766" requires scanning every tuple in the table — for a huge table, this is too slow.

An **index file** consists of **index entries**, each a `(search-key, pointer)` pair:
- **Search-key** — the attribute (or attribute set) values are looked up by.
- **Pointer** — the disk address of the actual record.

Index files are much smaller than the underlying data files, so they can often be kept in memory (fast) even when the data itself is too large to fit and must stay on disk (slow). Indexes trade write cost for read cost: every insert/delete/update on the table also has to update the index(es) built on it, so more indexes mean slower writes.

---

## Key Properties

### Storage and File Organization (Topic 19 — general DB theory, no slide/textbook source available)

A database's records are stored on disk as **files**, and how those files are physically organized determines how efficiently they can be accessed before any index is even involved.

- **Heap file (unordered):** records are placed wherever there is free space, in no particular order. Insertion is fast (append at the end); search requires a full linear scan (no ordering to exploit); deletion just marks space free (may need periodic reorganization to reclaim it).
- **Sorted file (ordered):** records are kept in physical order by some attribute. Search can use binary search (fast); insertion/deletion is expensive, since maintaining order may require shifting many records (or a fixed slot with occasional reorganization).

**Disk basics relevant to file organization:** data is transferred between disk and memory in fixed-size units called **blocks** (a.k.a. pages). A single disk read/write always moves a whole block, so **minimizing the number of block accesses** is the central cost concern in storage and indexing design — this is the same "reduce disk I/O" principle that shows up again in Query Processing.

- **Clustered (clustering) file organization:** records are physically stored grouped/ordered by a particular attribute's value — this is what makes a **primary index** possible (see below): the primary index's search key is defined as whichever key the table is physically clustered/sorted by.
- **Unclustered file organization:** records are not grouped by that attribute — any index built on such an attribute is necessarily a **secondary index**.

**Why this matters for indexing:** a table can have at most one clustering (physical sort) order — hence at most one primary index — but can have many non-clustering attributes indexed via secondary indexes. This is the direct link into the Primary vs. Secondary distinction below.

### Primary vs. Secondary Index

| | Primary Index | Secondary Index |
|---|---|---|
| Also called | Clustering index | Non-clustering index |
| Table ordering | Table is physically sorted by this index's search key | Table is **not** sorted by this index's search key |
| Count per table | Exactly 1 | Can be many |
| Usually built on | The primary key — **but not always** | Any frequently-queried non-key attribute |

**Common trap:** "primary index" does not mean "index on the primary key." It means "the index whose key the table is physically sorted by." A table can be sorted by a non-PK attribute, making that the primary index instead — which one is primary/secondary depends on which column the table's physical order matches, not on which column is the declared primary key.

### Dense vs. Sparse Index Files

| | Dense | Sparse |
|---|---|---|
| Coverage | Every search-key value gets an entry | Only some search-key values get entries |
| Valid for | Primary or secondary indexes | **Primary indexes only** |
| Space | More space, doesn't always fit in memory | Less space |
| Search speed | Faster (direct hit) | Slower (must scan forward from nearest entry) |
| Maintenance cost | Higher | Lower |

A sparse index only works because the table is physically sorted by that key (i.e., it can only be built on a primary index) — you can jump to the nearest entry at or before the target and scan forward. This is why sparse indexes can't be built on secondary keys: an unsorted table gives no "forward" to scan.

### Multilevel Indexing

A dense index can itself become too large to fit in memory. The fix: build a hierarchy —
- One **dense inner index** on disk, pointing to the actual table records.
- One or more **sparse outer index(es)** on top of the inner index, small enough to fit in memory.

This shrinks the search space searched on disk, cutting disk I/O. This hierarchical idea — sparse structure on top, dense structure at the bottom — is exactly what a B+ tree formalizes.

### B+ Tree — Structure

The dominant index structure in modern RDBMS (hash tables are used rarely, by comparison). A B+ tree is a **balanced, multi-level tree**:
- **Root/internal nodes** — act as the sparse outer index(es).
- **Leaf nodes** — act as the dense inner index; leaves are linked together (sequential scan support).
- Guarantees **logarithmic time complexity** for search, insert, and delete, regardless of data order.
- **Ordering invariant:** for a key in a node, all values in the subtree to its left are smaller, all values in the subtree to its right are ≥ it.

**Order "n" and the min/max rules** (n = max number of children an internal/root node can have):

| | Max | Min |
|---|---|---|
| **Children** | root/internal: n · leaf: 0 | Root: 2 (if root is not also a leaf) · Internal: ⌈n/2⌉ |
| **Keys/Values** | n − 1 | Root: 1 · Leaf and Internal: ⌈n/2⌉ − 1 |

> Some sources treat leaf and internal minimums as identical (as above); others give leaf min as ⌈(n−1)/2⌉. Slides use the first convention — apply it consistently.

### B+ Tree — Insertion

1. **Find the leaf** — walk down from root following the key comparisons until a leaf is reached.
2. **Insert if there's room** — if the leaf has fewer than max keys, insert in sorted order. Done.
3. **Split if full** — if the leaf is already at max, split it in half: first half → new left leaf, second half → new right leaf. Send the **lowest key of the new right node up to the parent**.
4. **Propagate upward if needed** — if the parent is now full too, split it the same way and push a key further up. This can continue up to the root; if the root itself splits, a **new root is created and the tree height increases**.

**Leaf split vs. non-leaf (internal) split — the key difference:** when a **leaf** splits, the separator key sent to the parent is *copied* — it stays in the new right leaf (leaves must hold the complete search-key set). When a **non-leaf** node splits, the separator key is *moved* up only — it is not duplicated in either child.

**Odd order (n) bias:** if n is odd, a split can't divide evenly, so it results in either a **left-biased** tree (left node gets 1 extra value) or a **right-biased** tree (right node gets 1 extra value). Whichever bias is chosen, **maintain it consistently across the entire tree**.

**String keys** sort in dictionary order, same as numeric keys sort by value.

### B+ Tree — Deletion

Deletion has 5 cases, driven by how many keys the target leaf, its siblings, and its parent have relative to the "min keys" rule above:

| Case | Condition | Action |
|---|---|---|
| **1** | Target leaf has > min keys | Just remove the key. (1a: key only in leaf. 1b: key also appears in an internal node — remove from leaf, then replace the internal-node copy with the **lowest value from its right-most subtree**.) |
| **2** | Target leaf = min keys, but a **sibling** has > min | **Borrow** from that sibling: the borrowed key moves through the parent (2a: borrow from left sibling, highest key moves up; 2b: borrow from right sibling, lowest key moves up). Internal-node copies are still fixed via the "lowest value from right-most subtree" rule. |
| **3** | Target leaf = min, sibling = min, but **parent** has > min | **Merge** target with its sibling, then remove the now-redundant parent key. |
| **4** | Target leaf = min, sibling = min, **parent also = min** (parent is an internal node) | Merge as in Case 3, then the **parent itself** either borrows a key from *its* sibling (4a) or merges with *its* sibling (4b) — the deficiency propagates one level up. |
| **5** | Same condition as Case 4, but resolving it **shrinks the tree height** | Merge propagates all the way to the root; if the root's own key is deleted (because it will be merged with its children), replace it with the lowest value from its right-most subtree *before* merging. |

**Rule to not forget:** any time a deleted key also exists as a copy in an internal node (Cases 1b, 2, 3+), that internal copy must be *replaced* with the lowest value from its right-most subtree — never just deleted and left blank.

### Hashing (brief — secondary structure, not the focus)

Some RDBMS use **hash tables** instead of B+ trees for indexes. Index entries are grouped into **buckets** (typically one disk block each), assigned by a hash function on the search key.

- **Bucket overflow** happens when: (a) multiple records share a search-key value, or (b) the hash function doesn't distribute keys uniformly. It can be reduced but never fully eliminated.
- **Handling overflow — forward/overflow chaining:** overflow buckets are linked together via a linked list off the primary bucket. This is the only method the slides mark as suitable for database use.
- Search can be O(1) in the best case, but degrades toward scanning every record in the worst case (poor hash function / heavy skew) — unlike a B+ tree's guaranteed logarithmic bound.

---

## Worked Example

**B+ tree insertion, order n = 4** (max keys/leaf = 3), inserting in sequence: `4, 10, 7, 12, 1, 5, 2, 25, 15, 9, 18, 20` (from slides):

1. Insert 4, 10, 7 → single leaf `[4, 7, 10]` (room for 3, no split yet).
2. Insert 12 → leaf full, **splits**: root becomes `[10]`, leaves `[4,7] | [10,12]`.
3. Insert 1 → fits in left leaf: `[1,4,7] | [10,12]`.
4. Insert 5 → left leaf full, **splits**: root `[5,10]`, leaves `[1,4] | [5,7] | [10,12]`.
5. Insert 2, 25 → both fit without splitting: `[1,2,4] | [5,7] | [10,12,25]`.
6. Insert 15 → right leaf full, **splits**: root `[5,10,15]`, leaves `[1,2,4] | [5,7] | [10,12] | [15,25]`.
7. Insert 9, 18 → both fit: `[1,2,4] | [5,7,9] | [10,12] | [15,18,25]`.
8. Insert 20 → rightmost leaf full, **splits**, and the **root itself is now full and splits too** → tree grows to height 2: new root `[15]`, with left subtree rooted at `[5,10]` (4 leaves) and right subtree rooted at `[20]` (leaves `[15,18] | [20,25]`).

This shows the full cascade: a leaf-only split (steps 2–6) escalating to a root split that increases tree height (step 8) — exactly the case worth practicing, since it's where the leaf-vs-non-leaf split distinction (see Key Properties) actually matters.

**Deletion, Case 1a example** (from slides, separate tree, n=4, min keys=1): deleting `7` where 7 only appears in a leaf `[5,7,8]` with 3 keys (> min of 1) — the key is simply removed, no borrowing or merging needed: `[5,8]`.

**More practice, with full worked solutions already available:** `CSE370_practice_ch11_indexing_hashing.pdf` —
- Q1: insert `8,5,1,7,3,12,9,6` into a B+ tree, n=3.
- Q2: same sequence, n=4.
- Q3: insert a 15-value sequence into n=4, plus a search-description exercise (search for 65, then a missing key 120 — useful for practicing how to correctly report "not found").
- Q5–Q7: three hash-index construction exercises (different hash functions, bucket sizes, forward-chaining), including a "compare which hash function is more uniform" question — directly matches the exam's likely angle on hashing.
- Q8: a **14-step deletion sequence** on one tree, showing every case escalate as the tree shrinks — the single best resource for deletion practice.

---

## Connections
- **Prerequisites:** none within CSE370's Final-scope topics (Topic 19, Storage & File Organization, is folded into this note above rather than being a separate prerequisite note).
- **Successors:** Query Processing and Optimization (Topic 22) — `CSE370_t22_query_processing_optimization.md`. Indexes are what query plans choose to use or not use; the same "minimize disk block accesses" principle from Topic 19 reappears there as the top query-optimization heuristic.

---

## Common Mistakes

- Assuming a heap (unordered) file organization allows binary search — it doesn't; binary search on file records requires a sorted file, same precondition as a binary-search scan in query processing.
- Confusing "clustered file organization" with "clustering index" as if they were separate concepts — a clustered file organization *is* what makes a table's primary/clustering index possible; they're the same underlying idea from two angles (storage layer vs. index layer).
- Confusing "primary index" with "index on the primary key" — the defining property is physical table ordering, not which column is the PK.
- Assuming sparse indexes can be built on secondary keys — they can't; sparse only works when the table is sorted by that key, which is a primary-index-only property.
- Getting the leaf-vs-internal split rule backwards during insertion: a **leaf** split *copies* the separator key up (it stays in the right leaf); a **non-leaf** split *moves* it up only (no duplicate left behind).
- Switching left/right bias partway through a tree when n is odd — pick one and hold it for every split.
- Misapplying the min-keys formula — root min children is 2 only when the root is *not* also a leaf (i.e., the tree has grown past a single node); leaf/internal min keys is ⌈n/2⌉ − 1, not ⌈n/2⌉.
- During deletion Cases 1b/2/3+, deleting an internal-node copy of a key outright instead of replacing it with the lowest value from its right-most subtree.
- Treating hashing as equal-weight with B+ trees for exam prep — slides explicitly frame it as the rarely-used alternative; B+ tree insertion/deletion is the higher-yield material.

---

## Anki Cards

START
Basic
What's the difference between a heap file and a sorted file, and what's the main tradeoff between them?
Back: Heap file = unordered, records placed wherever there's free space — fast insertion (append), but search requires a full linear scan. Sorted file = records physically ordered by an attribute — search can use binary search (fast), but insertion/deletion is expensive since order must be maintained.
END

START
Basic
Why does clustered vs. unclustered file organization directly determine whether an index on that attribute is primary or secondary?
Back: A primary/clustering index requires the table to be physically sorted by that key — that's exactly what "clustered file organization" on that attribute means. If the file isn't clustered by that attribute, any index built on it is necessarily secondary (non-clustering).
END

START
Basic
What is the defining property of a primary index, as opposed to being "an index on the primary key"?
Back: The table is physically sorted (ordered) by that index's search key. It's usually the primary key, but not always — physical ordering is the actual criterion.
END

START
Basic
Why can a sparse index only be built on a primary index, never a secondary one?
Back: Sparse indexes rely on scanning forward from the nearest indexed entry, which only works if the table is physically sorted by that key — which is exactly what makes an index "primary."
END

START
Basic
In a B+ tree of order n, what is the max number of keys in any node, and the min number of keys in a leaf/internal node?
Back: Max = n − 1. Min (leaf and internal, by the slides' convention) = ⌈n/2⌉ − 1. Root min keys = 1.
END

START
Basic
When a leaf node splits during B+ tree insertion, what happens to the separator key sent to the parent?
Back: It is copied — the key stays in the new right leaf as well as appearing in the parent. (Contrast: a non-leaf/internal split moves the key up without duplicating it in the child.)
END

START
Basic
In B+ tree deletion, when a deleted key also appears as a copy in an internal node, what must replace that copy?
Back: The lowest value from that key's right-most subtree — never just an empty removal.
END

START
Basic
What is the only overflow-handling method for hash indexing that the slides mark as suitable for database use, and why does overflow happen at all?
Back: Forward/overflow chaining (linked overflow buckets). Overflow happens because multiple records can share a search-key value, or because the hash function doesn't distribute keys uniformly — it can be reduced but never fully eliminated.
END

START
Basic
B+ tree deletion Case 3 vs Case 4 — what's the difference in trigger condition?
Back: Case 3: target leaf and its sibling are both at min keys, but the parent has more than min keys (just merge and remove one parent key). Case 4: the parent is also at min keys, so the parent itself must then borrow from or merge with its own sibling.
END
