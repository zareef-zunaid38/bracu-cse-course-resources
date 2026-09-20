---
source: CSE370_practice_ch11_indexing_hashing.pdf (the original, included in this repo)
fidelity: text pages = full text-layer extraction; figure pages = described (data tables and charts transcribed, photos/diagrams summarised) — open the PDF page for detail
figure-pages: 4, 5, 6, 7, 12, 13
source-mtime: 2026-07-31T22:33:10
generated: 2026-09-20
---
# CSE370_practice_ch11_indexing_hashing.pdf

<!-- p.1 -->
CSE370 : Database Systems
Practice Sheet
Indexing and Hashing
*Solutions are at the end of the document. SOLVE IT YOURSELF first.
Question 1:
An EMPLOYEE file with ID# as the key field includes records with the following ID# values: 8, 5, 1, 7,
3, 12, 9, 6. Suppose that the search field values are inserted in the given order in a B+-tree of order n = 3;
show how the tree will expand and what the final tree will look like.
Question 2:
An EMPLOYEE file with ID# as the key field includes records with the following ID# values: 8, 5, 1, 7,
3, 12, 9, 6. Suppose that the search field values are inserted in the given order in a B+-tree of order n = 4;
show how the tree will expand and what the final tree will look like.
Question 3:
A. An EMPLOYEE file with ID# as the key field includes records with the following ID# values:
15, 5, 20, 10, 65, 55, 60, 50, 30, 25, 35, 90, 85, 95, 105, 100, 110, 115. Suppose that the search
field values are inserted in the given order in a B+-tree of order n = 4; show how the tree will
expand and what the final tree will look like.
B. After building the tree in this sequence, describe in your words how you will search for these
values in the tree: 65 and 120.
Question 4:
A. Discuss the similarities and differences between indexing and hashing.
B. Compare and contrast primary and secondary indexing with examples
C. What is multilevel indexing? Discuss the necessity of multilevel indexing.
D. Compare and contrast sparse indexing and dense indexing.

<!-- p.2 -->
Question 5:
Suppose you have a table titled ‘“Instructor”.
A. Make a hash index on attribute instructor_id. Suppose the number of buckets in the hash
index is 5 and each bucket can house 2 index entries at max. Bucket overflow is handled
using forward chaining.
The hash function, h = (sum of all digits in id) % 5
B. Explain how to search for the following search keys in your hash index.
●
45565
●
11113
Question 6:
Suppose you have a table titled ‘“Instructor”.
Make a hash index on attribute instructor_name (2nd column). Suppose the number of buckets
in the hash index is 5 and each bucket can house 3 index entries at max. Bucket overflow is
handled using forward chaining. The values of each character from A-Z (or a-z) is 1-26
respectively
The hash function, h = (sum of the values of each character in name) % 5
Ex:
h(Wu) = (23 + 21) % 5 = 4

<!-- p.3 -->
Question 7:
Suppose you have the table below for “Instructors” for a School Database with columns ID,
Name, Subject and Salary respectively:
Suppose the number of buckets in the hash index is 5 and each bucket can hold 3 index entries at max.
Construct a hash index on attribute ID. Bucket overflow is handled using forward chaining. The hash
function, h = (First digit of ID) % 5.
Now let's assume there is another hash function, h = (Last digit of ID) % 5.
Now compare which hash function is more uniform.
Question 8:
Given the B+ tree below for n= 4, delete the following values in the given sequence. After each deletion
show the updated tree.
35, 90, 95, 55, 25, 30, 20, 100, 50, 50, 65, 60, 100, 115, 85

<!-- p.4 -->
SOLUTIONS:
Answer 1:
> **[Figure, PDF p.4]** Answer 1 (solution): a stack of B+ tree diagrams showing the tree after each successive insertion, from a single leaf [0008] growing to a multi-level tree; final tree has root [0007]. Search keys seen: 0001 0003 0005 0007 0008 0009 0012. Every intermediate tree is only in the PDF page.

<!-- p.5 -->
Answer 2:
> **[Figure, PDF p.5]** Top: final tree of Answer 1 (root [0007]). 'Answer 2': another stack of B+ tree states from [0008] upward with the same key set; ends with root [0007 0009]. Every intermediate tree is only in the PDF page.

<!-- p.6 -->
Answer 3:
A.
> **[Figure, PDF p.6]** Top: final tree of Answer 2 (root [0005 0007 0009], leaves [0001 0003] [0005 0006] [0007 0008] [0009 0012]). 'Answer 3, part A': B+ tree states for keys 0005 0010 0015 0020 0055 0060 0065; ends with root [0015 0055].

<!-- p.7 -->
> **[Figure, PDF p.7]** Continuation of Answer 3: six B+ tree states with keys up to 0090 (0005 0010 0015 0020 0025 0030 0035 0050 0055 0060 0065 0085 0090); the last two have single-key root [0055] with internals [0015 0030] and [0065].

<!-- p.8 -->
B. Searching 65:
Start at root with values 55, 100. 65 is RHS of 55 which has values 65 and 90, so go to RHS of 65
and we reach the leaf, the first value is 65.
Searching 120:
Start at root with values 55, 100. 120 is RHS of 100, where the node value is 110. 120 is again in
the RHS of node 110, we reach the leaf node. The values are 110 and 115, so 120 does not exist.

<!-- p.9 -->
Answer 4:
Take help from slides, books and online materials to write a concise answer to each question in your own
words.
Answer5:
A.

<!-- p.10 -->
B:
Searching for 45565:
h(45565) = 0
Therefore, we examine the buckets located at index 0. Upon retrieving this initial bucket, we
encounter two entries, namely 22222 and 32343. As our intended record has not been found,
we proceed to retrieve the second bucket, from which we obtain the entry 45565.
Searching for 11113:
h(11113) = 2
Therefore, we examine the buckets located at index 2. Upon retrieving the initial bucket, two
records, namely 12121 and 76766, are obtained. Subsequently, retrieving the second bucket
yields the entry 83821. At this point, all buckets at index 2 have been retrieved, and the end of
the linked list has been reached. Consequently, the search for the key 11113 concludes without
success.
Answer 7:

<!-- p.11 -->
The 1st hash function is more uniform compared to the later one. As no buckets were empty in the 1st
case. However, in the 2nd case not only were there empty buckets but also there was an overflow in the
bucket.
Answer 8:

<!-- p.12 -->
> **[Figure, PDF p.12]** Six successive B+ tree states over keys 0005-0115 (first root [0050 0100]); a sequence of deletions/insertions whose exact operation order is stated in the question text on earlier pages.

<!-- p.13 -->
> **[Figure, PDF p.13]** Four further B+ tree states; final tree: root [0015 0105], leaves [0005 0010] [0015] [0105].
