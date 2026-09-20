---
source: CSE370_Lecture_08_indexing_hashing.pdf (the original, included in this repo)
fidelity: full — text transcribed slide by slide from renders (the PDF has no text layer); tree and index diagrams are transcribed as node lists, hand-marked diagrams on slides 22 and 24 may lose pointer detail
figure-pages: 2, 4, 6, 7, 8, 10-27, 28, 29, 30
source-mtime: 2026-07-18T12:20:54
generated: 2026-09-20
---
# CSE370 Lecture 8: Database Indexing & Hashing

BRAC University CSE370: Database Systems slide deck, 30 slides (the title slide reads "Lecture 7: Database Indexing"; filed as Lecture 8). Clean digital slides.
Rewritten 2026-09-20 from all 30 slides viewed one by one: the earlier version said Cases 1 and 3–4 of B+ tree deletion were "not sampled" and gave a different insertion example than the deck's. Diagrams of trees are transcribed as node lists; see the PDF page for exact tree drawings.

## Content

<!-- p.1 -->
**Slide 1 — title:** "CSE370: Database Systems", "Lecture 7: Database Indexing"; a Key | Pointer table with entries A–E pointing to rows of a data table.

<!-- p.2 -->
**Slide 2 — Indexing:** many queries touch only a small share of a table's records, and reading every tuple (e.g. to find DeptName "Physics" or ID "76766") is inefficient on a huge database. **Indexes** are extra data structures that speed up access and give logarithmic search time even over unsorted data. Index files are much smaller than the data files, so they can sit in memory (disk I/O is slower, memory is limited) for faster access. Indexes can be created on any attribute used frequently in search queries or joins; insert/delete/update becomes slower because the indexes must be updated too. Example table Instructor (ID, Name, DeptName, Salary): 10101 Srinivasan Comp. Sci. 65000; 12121 Wu Finance 90000; 15151 Mozart Music 40000; 22222 Einstein Physics 95000; 32343 El Said History 60000; 33456 Gold Physics 87000; 45565 Katz Comp. Sci. 75000; 58583 Califieri History 62000; 76543 Singh Finance 80000; 76766 Crick Biology 72000; 83821 Brandt Comp. Sci. 92000; 98345 Kim Elec. Eng. 80000.

<!-- p.3 -->
**Slide 3 — Types of indexes:** *Primary index* (also "clustering index"): the table is physically ordered (sorted) by the primary index search key; only one per table, usually the primary key (but not always). *Secondary index* ("non-clustering index"): the table is not ordered by the secondary index search key; several per table.

<!-- p.4 -->
**Slide 4 — Examples (primary & secondary):** ID and Department are indexes in both tables below. Table 1 rows (ID, Department): 1 EEE, 2 CSE, 3 BBS, 4 CSE — sorted by ID, so ID is the primary index and Department the secondary. Table 2 rows: 3 BBS, 2 CSE, 4 CSE, 1 EEE — sorted by Department, so there ID is secondary and Department is primary.

<!-- p.5 -->
**Slide 5 — Types of index files:** index files consist of records (index entries) of the form Search-key | Pointer, where the search key is the value of an attribute (or set) used to look up records and the pointer is the memory address of the original data on disk. **Dense index file:** every search-key value has an entry; can be used for both secondary and primary indexes; needs more space (may not fit in main memory) but searches faster; higher maintenance overhead for insertion/deletion. **Sparse index file:** only some search-key values appear; only for primary indexes; reduces space but searching is slower than with dense files; lower maintenance overhead for insertion/deletion.

<!-- p.6 -->
**Slide 6 — Examples (dense):** Figures 1 and 2 show dense index files on the primary index "ID" and "Department_Name" respectively (one entry per instructor row); Figure 3 shows a dense index file on a secondary index "Salary" (an index over sorted salaries 40000…95000 with pointers to the unsorted rows).

<!-- p.7 -->
**Slide 7 — Examples (sparse):** Figures 1 and 2 show sparse index files on the primary index "ID" (entries 10101, 32343, 76766 pointing into the sorted instructor table) and "Department_Name" (entries Biology, Finance, Physics pointing into the table sorted by department). Sparse files can only be created on primary indexes.

<!-- p.8 -->
**Slide 8 — Multilevel indexing:** a dense index may not fit in main memory for large databases, and reading it from disk costs I/O; the solution is a hierarchical, multi-level index. One dense *inner index* is stored on disk and points to the original records; one or more sparse *outer index(es)* sit on top of it and, being smaller, can be stored in memory — reducing the search space in the inner index and the disk I/O. Diagram: outer index (sparse) Alice 1102, Bob 1103, Christie 1104 → second-level outer index (sparse) Alice 1202, Ben 1203, Bethany 1204 and Billie 1500, Bob 1501, Charlie 1502 → inner index (dense) blocks Alice/Adrian, Ben/Benjamin, Bethany/Betty.

<!-- p.9 -->
**Slide 9 — Data structures for indexing:** the most common structure for indexing in modern relational DBMS is the **B+ tree**; rarely some RDBMS use **hash tables**. A B+ tree is a multi-level index where root/internal nodes are sparse outer indexes and leaf nodes form the dense inner index; B+ trees are balanced, giving logarithmic search time. Hash tables can search in constant time, but that depends on the hash function's "goodness": if it does not distribute keys uniformly, in the worst case every record might be searched.

<!-- p.10 -->
**Slide 10 — B+ tree properties:** every tree has an order "n" that fixes the min/max number of children and keys per node; nodes to the left of a key hold smaller values, nodes to the right hold values greater than or equal. Table — *Children:* max = n for root/internal, 0 for leaf; min = 2 for the root (if the root is not also a leaf), ⌈n/2⌉ for internal nodes. *Keys/values:* max = n−1; min = 1 for the root, ⌈n/2⌉−1 for leaf and internal nodes (note on slide: for simplicity leaf and internal are treated the same in some resources, otherwise the leaf minimum is ⌈(n−1)/2⌉).

<!-- p.11 -->
**Slide 11 — B+ tree insertion (steps):** *Find the right spot (leaf):* start at the root and follow the path for the key until a leaf. *Insert if the node has fewer than "max" keys:* insert in sorted order. *Split if the node has "max" keys:* split the full leaf in half, first half on a new left leaf and second half on a new right leaf, and send up the lowest key of the new right node to the parent. *Repeat the split upwards if needed:* if the parent becomes full, split it too and push a key further up; if a non-leaf node is split, the lowest value on the right is only moved up, not copied to the new right leaf; this may continue to the root, and if the root splits a new root is created and the height increases. Diagram: leaf [0004 0007 0010] → after inserting 0012, root [0010] over leaves [0004 0007] and [0010 0012].

<!-- p.12 -->
**Slide 12 — Leaf vs non-leaf split (for n = 4):** leaf split — root [0010] over leaves [0004 0007] → [0010 0012] (the 0010 is copied up and remains in the right leaf); non-leaf split — root [0010] over [0004 0007] and a right node holding only [0012] (the 0010 is moved up, not kept in the right node).

<!-- p.13 -->
**Slide 13 — odd n (e.g. n = 3):** splitting gives either a left-biased or a right-biased tree. Right-biased: the right node holds 1 value more than the left; left-biased: the left node holds 1 more; keep the same bias for the whole tree. Diagram: left bias — root [0010] over leaves [0005 0008] and [0010]; right bias — root [0008] over leaves [0005] and [0008 0010].

<!-- p.14 -->
**Slide 14 — string keys:** strings are sorted in dictionary order. Kim, Kate and Ken are inserted sequentially in a right-biased tree for n = 3: [Kim] → [Kate Kim] → root [Ken] over leaves [Kate] and [Ken Kim].

<!-- p.15 -->
**Slide 15 — insertion exercise:** insert into a B+ tree of order n = 4 in this sequence: 4, 10, 7, 12, 1, 5, 2, 25, 15, 9, 18, 20 (a link to a B+ tree simulator is given). Max keys in a node = 4 − 1 = 3. *Insert 4, 10, 7:* leaf [0004 0007 0010]. *Insert 12:* root [0010] over leaves [0004 0007] and [0010 0012]. *Insert 1:* root [0010] over leaves [0001 0004 0007] and [0010 0012].

<!-- p.16 -->
**Slide 16 — insertion (cont.):** *Insert 5:* root [0005 0010] over leaves [0001 0004], [0005 0007], [0010 0012]. *Insert 2, 25:* root [0005 0010] over leaves [0001 0002 0004], [0005 0007], [0010 0012 0025]. *Insert 15:* root [0005 0010 0015] over leaves [0001 0002 0004], [0005 0007], [0010 0012], [0015 0025].

<!-- p.17 -->
**Slide 17 — insertion (cont.):** *Insert 9, 18:* root [0005 0010 0015] over leaves [0001 0002 0004], [0005 0007 0009], [0010 0012], [0015 0018 0025]. *Insert 20:* root [0015] over internal nodes [0005 0010] and [0020]; leaves under [0005 0010]: [0001 0002 0004], [0005 0007 0009], [0010 0012]; leaves under [0020]: [0015 0018], [0020 0025].

<!-- p.18 -->
**Slide 18 — B+ tree deletion (the five cases):** if the value exists, the target node falls under one of five cases (a simulator link is given). **Case 1:** number of keys > "min" keys allowed. **Case 2:** target has exactly "min" keys but a sibling has more than "min". **Case 3:** target and siblings have "min" keys, but the parent has more than "min" keys. **Case 4:** target, siblings and parent (internal nodes) all have "min" keys. **Case 5 (special case):** same condition as Case 4; the special part is that the tree shrinks in height.

<!-- p.19 -->
**Slide 19 — Case 1a (target value only in a leaf): delete 7.** For n = 4, leaf/internal min keys = ⌈n/2⌉ − 1 = ⌈4/2⌉ − 1 = 1. Find the key in the leaf and remove it. Tree before: root [0015]; internal [0005 0010] and [0025]; leaves [0002 0004], [0005 0007 0008], [0010 0012], [0015 0020], [0025 0035]. After: same tree, leaf [0005 0008].

<!-- p.20 -->
**Slide 20 — Case 1b (value in a leaf and an internal node): delete 15.** Find the key in the leaf and remove it, then find it in the internal node and replace it with the lowest value of its right-most subtree. Before: root [0015], internals [0005 0010] and [0025]; leaves [0002 0004], [0005 0008], [0010 0012], [0015 0020], [0025 0035]. After: root [0020]; internals [0005 0010] and [0025]; leaves [0002 0004], [0005 0008], [0010 0012], [0020], [0025 0035].

<!-- p.21 -->
**Slide 21 — Case 2a (left sibling has more than "min"): delete 8.** Find the key in the leaf; the highest key of the left sibling is copied to the parent and to the target node; for internal nodes, replace the value with the lowest value from the right-most subtree. Before: root [0020]; internals [0008 0010] and [0025]; leaves [0002 0004], [0008], [0010], [0020], [0025 0035]. After: root [0020]; internals [0004 0010] and [0025]; leaves [0002], [0004], [0010], [0020], [0025 0035].

<!-- p.22 -->
**Slide 22 — Case 2b (right sibling has more than "min"): delete 20.** Find the key in the leaf; the lowest key of the right sibling is copied to the target node; the parent value is replaced by the lowest value from the right-most subtree; for internal nodes likewise. Before: root [0020]; internals [0004 0010] and [0025]; leaves [0002], [0004], [0010], [0020], [0025 0035]. After: root [0025]; internals [0008 0010] and [0035]; leaves [0002 0004], [0008], [0010], [0025], [0035]. (The slide's diagram is partly hand-marked; exact pointers are in the PDF page.)

<!-- p.23 -->
**Slide 23 — Case 3: delete 10.** Target leaf and its sibling are at "min" but the parent has more than "min": find the key in the leaf, remove it, merge the target with its sibling, and remove the parent key/value; for internal nodes, replace with the lowest value from the right-most subtree. Before: root [0025]; internals [0008 0010] and [0035]; leaves [0002 0004], [0008], [0010], [0025], [0035]. After: root [0025]; internals [0008] and [0035]; leaves [0002 0004], [0008], [0025], [0035].

<!-- p.24 -->
**Slide 24 — Case 4a (internal parent borrows from sibling): delete 12.** Target leaf, sibling and parent are all at "min"; the parent (an internal node) will either borrow from its sibling or merge with it. Case 4a: find the key in the leaf, remove and merge with the sibling as in Case 3; the internal parent borrows 1 key from a sibling — the sibling passes the key up to its parent and that parent passes its old value down to the internal node. Before: root [0008 0025]; internals [0003 0005], [0012], [0035]; leaves [0001 0002], [0003 0004], [0005 0007], [0008], [0012], [0025], [0035]. After: root [0005 0025]; internals [0003], [0008], [0035]; leaves [0001 0002], [0003 0004], [0005 0007], [0008], [0025], [0035].

<!-- p.25 -->
**Slide 25 — Case 4b (internal parent merges with sibling): delete 35.** Find the key in the leaf, remove and merge with the sibling as in Case 3; the internal parent merges with one of its siblings and their parent key. Before: root [0005 0025]; internals [0003], [0008], [0035]; leaves [0001 0002], [0003 0004], [0005 0007], [0008], [0025], [0035]. After: root [0005]; internals [0003] and [0008 0025]; leaves [0001 0002], [0003 0004], [0005 0007], [0008], [0025].

<!-- p.26 -->
**Slide 26 — Case 5 (shrinking the tree): delete 25.** Case 5 and Case 4 conditions are exactly the same — target leaf, sibling and parent all at "min"; the internal parent merges with its sibling and the tree height shrinks. Find the key in the leaf, remove and merge with the sibling as in Case 3; the internal parent merges with one of its siblings and their parent key. Before: root [0007]; internals [0003] and [0025]; leaves [0001 0002], [0003 0004], [0007], [0025]. After: root [0003 0007]; leaves [0001 0002], [0003 0004], [0007].

<!-- p.27 -->
**Slide 27 — Case 5, another example: delete 7.** Same process as the previous example, but since the key is also present in the root node, which will be merged with its children, the root value is replaced with the lowest value from the right-most subtree first and then merged. Before: root [0007]; internals [0003] and [0025]; leaves [0001 0002], [0003 0004], [0007], [0025]. After: root [0003 0025]; leaves [0001 0002], [0003 0004], [0025].

<!-- p.28 -->
**Slide 28 — Static hashing:** some RDBMS use hash tables to store indexes. One or more index entries are stored in a **bucket** (typically a disk block); the bucket of an entry is found by applying a hash function to its search key. Entries with different search keys may map to the same bucket, so the whole bucket has to be searched sequentially. **Bucket overflow** can occur because of too few buckets or skew in the distribution of records (multiple records with the same search-key value, or a hash function that gives a non-uniform distribution). Overflow probability can be reduced but not eliminated; it is handled with *forward chaining / overflow chaining* (overflow buckets chained together in a linked list); other overflow-resolution methods are not suitable for database applications. Diagram: buckets 0–3 with overflow buckets chained off bucket 1.

<!-- p.29 -->
**Slide 29 — Hashing example:** hash function h(ID) = (sum of digits in ID) % 8, modulo 8 because there are 8 hash buckets. Hash index on `instructor`, attribute ID: bucket 0 — 76766; bucket 1 — 45565, 76543; bucket 2 — 22222; bucket 3 — 10101; bucket 4 — empty; bucket 5 — 15151, 33456, with overflow bucket 58583, 98345; bucket 6 — 83821; bucket 7 — 12121, 32343. Each entry points to its row of the instructor table (76766 Crick Biology 72000, 10101 Srinivasan Comp. Sci. 65000, 45565 Katz Comp. Sci. 75000, 83821 Brandt Comp. Sci. 92000, 98345 Kim Elec. Eng. 80000, 12121 Wu Finance 90000, 76543 Singh Finance 80000, 32343 El Said History 60000, 58583 Califieri History 62000, 15151 Mozart Music 40000, 22222 Einstein Physics 95000, and a last row printed "33465 Gold Physics 87000" — the bucket diagram shows 33456 in bucket 5, so the table row is probably the same record with a transposed digit on the slide).

<!-- p.30 -->
**Slide 30 — "What Next?":** teaser "Lecture 8: Transactions — loading…" with the transaction state diagram (Begin → Active → Partially Committed → Committed → End; Active → Failed → Aborted → End).
