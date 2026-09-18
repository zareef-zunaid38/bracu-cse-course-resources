# CSE370 Lecture 8: Database Indexing & Hashing — sidecar (read this, not the .pdf)

BRAC University CSE370: Database Systems slide deck, 30 slides (titled "Lecture 7:
Database Indexing" on the title slide, filed as Lecture 8). Clean digital slides, fully
legible — transcribed with confidence.

## Content

**Indexes basics:** an index entry = `Key | Pointer` (pointer = memory/disk address of the
actual record). Primary/clustering index: table is physically sorted by this key; only 1 per
table, usually the primary key. Secondary/non-clustering index: table not sorted by this
key; multiple allowed per table. Worked example: a table with `ID, Name, Department` columns
— sorting by ID makes ID primary/Department secondary; sorting the same data by Department
instead makes Department primary/ID secondary (which attribute is "primary" depends on
which one the table is physically sorted by, not which column it is).

**Dense vs. sparse index files:** Dense = every search-key value gets an index entry; works
for both primary and secondary indexes; more space, faster search, higher insert/delete
maintenance overhead. Sparse = only some search-key values indexed; primary indexes only;
less space, slower search, lower maintenance overhead. Worked examples (Figures 1 & 2) show
sparse indexes on primary key "ID" and on primary key "Department_Name" respectively —
sparse files can only be built on primary (sorted) indexes, since you need the sort order to
know which records a given index entry's "range" covers.

**Data structures for indexing:** modern RDBMS mostly use **B+ trees** (rarely, hash tables).
B+ tree = multi-level index structure; root/internal nodes act as a sparse outer index,
leaf nodes form the dense inner index; balanced tree → logarithmic search time. Hash tables
can offer constant-time search, but only if the hash function distributes keys uniformly —
a bad hash function degrades to searching every record in the worst case.

**B+ Tree insertion algorithm:**
1. Find the right leaf node by walking down from the root following the key.
2. If the leaf has room (< max keys), insert in sorted order.
3. If the leaf is full (= max keys), split into two leaves (first half left, second half
   right), and push the lowest key of the new right leaf up to the parent.
4. If the parent then overflows too, repeat the split upward (for a non-leaf split, the
   pushed-up key is *moved*, not copied, unlike a leaf split) — may cascade to the root,
   creating a new root and increasing tree height.
   - Worked example given: sequential inserts of 5, then 2 & 25, then 15 into a small B+
     tree, showing leaf-level splits and the growing linked-list of leaves.
   - Odd-order trees (e.g. n=3): splitting is either **left-biased** (left node keeps 1 more
     key) or **right-biased** (right node keeps 1 more) — pick one convention and keep it
     consistent for the whole tree.

**B+ Tree deletion — 5 cases** (each with a worked before/after diagram):
- **Case 1** (not directly captured in sampled slides, but implied): target leaf has more
  than the minimum number of keys — just remove the key, no rebalancing needed.
- **Case 2 (borrow from sibling):** target leaf is at the minimum, but a sibling has more
  than the minimum — borrow from the sibling. Case 2a (left sibling has extra): the
  sibling's highest key is copied up to the parent and into the target node. For internal
  (non-leaf) nodes, replace the removed value with the lowest value from the right subtree
  instead. Worked example: deleting 8 from a tree where the left sibling leaf has an extra
  key.
- **Cases 3-4** (not directly sampled, but per Case 5's own description "Case 5 and Case 4
  conditions are the exact same"): target leaf, its sibling, and the parent are all at the
  minimum — merge target with sibling (Case 3 covers the leaf-level merge only; Case 4 is
  the same condition applied where merging cascades to an internal parent too).
- **Case 5 (shrinking the tree):** same condition as Case 4, but the internal parent node
  itself ends up merging with one of its siblings and absorbing their shared parent key,
  which shrinks the overall tree height by one level. Worked example: deleting 25 causes a
  leaf merge AND an internal-node merge, reducing a 3-level tree to 2 levels.

**Static hashing:** some RDBMS store indexes in hash tables instead of B+ trees. One or more
index entries are stored per **bucket** (typically a disk block); a hash function maps a
search-key to its bucket. Entries with different keys can collide into the same bucket, so
the whole bucket must be searched sequentially once you're in it. **Bucket overflow** happens
from insufficient buckets or skewed key distribution (either duplicate search-key values, or
a non-uniform hash function) — can't be eliminated entirely, only mitigated, via **overflow
chaining** (overflow buckets linked together in a chain off the original bucket) — described
as the only method suitable for database applications (other general hashing overflow
strategies aren't).

**Closing slide:** teaser for "Lecture 8: Transactions" — includes the standard transaction
state-diagram (Active → Partially Committed → Committed / Active → Failed → Aborted, via
Begin/End).
