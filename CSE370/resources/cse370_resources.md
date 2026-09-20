---
course: CSE370
type: resource-guide
status: complete
---

# CSE370 — Resources

## Textbooks (from Course Outline — Section F)

Three reference books listed. Order in the outline is the official order.

| # | Title | Authors | Edition | Year | Publisher |
|---|---|---|---|---|---|
| 1 | Fundamentals of Database Systems | Elmasri & Navathe | 7th ed. | 2015 | Pearson |
| 2 | Database Systems: A Practical Approach | Connolly & Begg | 6th ed. | 2014 | Pearson |
| 3 | Database Systems Concepts | Silberschatz, Korth & Sudarshan | 6th ed. | 2011 | McGraw-Hill |

> [!warning] Verified 2026-09-06 (concept-note audit): both local textbook PDFs are **short front-matter excerpts, not the full books.** Silberschatz is 94 pages and ends mid-Chapter 3 (SQL basics); Elmasri & Navathe is 48 pages and ends mid-Chapter 1. Neither contains any of Ch 10 onward (Silberschatz) or Ch 3 onward (E&N) — so every topic-by-topic chapter citation below Storage & Indexing is currently **unreadable from the local copies**, despite being the correct chapter per the course outline mapping. Do not cite these chapters as an actually-consulted source in a concept note without first re-checking the local file (a fuller copy may be obtained later) — `CSE370_t19_indexing.md`, `CSE370_t23_transactions.md`, `CSE370_t24_concurrency_control.md`, and `CSE370_t22_query_processing_optimization.md` all had this citation error and were corrected on this date.

> [!note] Textbooks (not hosted in this repo):
> - `Silberschatz_Korth_Sudarshan_Database_System_Concepts_6e.pdf` — excerpt only, see warning above
> - `Elmasri_Navathe_Fundamentals_of_Database_Systems_7e.pdf` — excerpt only, see warning above
> - Connolly & Begg 6e — not yet obtained.

> [!note] Prior versions of this file incorrectly listed Silberschatz 7th ed. as the sole primary textbook. The outline lists it third at 6th ed. Elmasri & Navathe is listed first. Topic-by-topic notes below may reference Silberschatz chapter numbers — treat those as approximate until confirmed with NFF.

## Lab Environment

**XAMPP** — Apache + MySQL + PHP stack. MySQL is accessed via the XAMPP shell.
Download: https://www.apachefriends.org/index.html
Default MySQL root password in XAMPP: empty string (just press Enter).

**SQLZoo** — https://sqlzoo.net — browser-based SQL practice, no setup required.
Use for SELECT and JOIN practice after each SQL topic.

## Lecture Slides

**Location:** `CSE370/slides/`

> [!note] Filenames follow slide-download order, not confirmed exam-syllabus order. Cross-check `cse370_exam_notes.md` for what was actually examined before treating any file below as "next" or "not yet covered" — do not infer scope from the topic map or this table alone.

| File | Topics Covered | Exam status |
|---|---|---|
| `CSE370_Lecture_01_intro_databases.pdf` | Topic 1: Introduction to Databases | Midterm |
| `CSE370_Lecture_02_er_model.pdf` | Topic 2: Data Modeling Using ER Model | Midterm |
| `CSE370_Lecture_03_eer.pdf` | Topic 3: Enhanced Entity Relationships (EER) | Midterm |
| `CSE370_Lecture_04_relational_constraints.pdf` | Topic 4: Relational Database Constraints | Midterm |
| `CSE370_Lecture_05_relational_db_design.pdf` | Topics 13–14: ER→Relational, Functional Dependencies | Midterm |
| `CSE370_Lecture_06_normalization.pdf` | Topics 14–17: 1NF through BCNF | Midterm |
| `CSE370_Lecture_relational_algebra.pdf` | Topic 10: Relational Algebra | Midterm (this was the actual 7th lecture — see `cse370_exam_notes.md`) |
| `CSE370_Lecture_08_indexing_hashing.pdf` | Topics 19–21: Storage, Indexing, B+ Trees | **Not on Midterm** — first confirmed post-Midterm material. Renamed 2026-07-31 from `CSE370_Lecture_07_database_indexing.pdf`, which wrongly implied it was the tested 7th lecture. |
| `CSE370_Lecture_09_transactions.pdf` | Topic 23: Transaction Management | Not on Midterm. **Corrected 2026-09-06:** the 6-page/30-page discrepancy flagged 2026-09-03 is resolved — verified the local copy is the full 30-page version (page-count + content check); the Downloads duplicate no longer exists. |
| `CSE370_Lecture_10_concurrency_control.pdf` | Topic 24: Concurrency Control | Not on Midterm. Added 2026-09-03. Confirmed Quiz 4 syllabus (2026-09-07), alongside Lecture 9. |
| `CSE370_Lecture_11_query_processing.pdf` | Topic 22: Query Processing and Optimization (part 1) | Not on Midterm. Added 2026-09-03. |
| `CSE370_Lecture_12_query_optimization.pdf` | Topic 22: Query Processing and Optimization (part 2) | Not on Midterm. Added 2026-09-03. |
| `CSE370_Lecture_12_heuristic_optimization_supplement.pdf` | Topic 22: Heuristic Query Optimization — condensed 11-slide handout, same Aquarius worked example as the main Lecture 12 deck. **Added 2026-09-13**, provided directly by NFF ("additional slide I used in class"). Clean text PDF, no separate text version needed. | Not on Midterm. Confirmed used in the actual class session for this material — treat as authoritative alongside `CSE370_Lecture_12_query_optimization.pdf`. |

> [!missing] Slides for Topics 5–9 (Relational Model, SQL) not yet downloaded from Brightspace.

## Practice Sheets

**Location:** `CSE370/practice/`

| File | Chapter / Topic |
|---|---|
| `CSE370_practice_ch01_intro_db.pdf` | Ch 1: Introduction to Databases (Topic 1) |
| `CSE370_practice_ch03_er.pdf` | Ch 3: ER Model (Topics 11–12) |
| `CSE370_practice_ch04_eer.pdf` | Ch 4: EER Model (Topics 11–12 extended) |
| `CSE370_practice_ch05.pdf` | Ch 5 (content TBD — open and verify) |
| `CSE370_practice_ch07_relational_schema.pdf` | Ch 7: Relational Schema (Topic 13) |
| `CSE370_practice_ch10_normalization.docx` | Ch 10: Normalization (.docx) |
| `CSE370_practice_ch10_normalization.pdf` | Ch 10: Normalization (PDF version, obtained 2026-07-29) |
| `CSE370_practice_ch08_relational_algebra.pdf` | Ch 8: Relational Algebra (Topic 10) — obtained 2026-07-29 |
| `CSE370_practice_ch11_indexing_hashing.pdf` | Ch 11: Indexing and Hashing (Topics 19–21) — **Final-track, not Midterm** — obtained 2026-07-29. **Renamed 2026-09-03** from `CSE370_practice_ch13_indexing_hashing.pdf` — verified against the actual Silberschatz 6e table of contents (Ch 13 is Query Optimization, not this topic). All references updated. |
| `CSE370_practice_ps9_transactions.pdf` | Ch 14: Transactions (Topic 23) — obtained 2026-08-18. **Updated 2026-09-13** per official Final Notice ("new problems added to Lecture 9's practice sheet") — 17→19 questions, Q1–17 unchanged (verified against `CSE370_t23_transactions.md`/`CSE370_t24_concurrency_control.md` citations, no renumbering), new Q18–19 apply the Timestamp-Ordering MVCC algorithm — directly relevant now that MV2PL is confirmed out of scope. |
| `CSE370_practice_ch12_query_processing.md` | Ch 12: Query Processing (Topic 22) — obtained 2026-09-03. A 2026-09-03 Downloads duplicate of `CSE370_practice_ps9_transactions.pdf` (byte-identical text, different export metadata) was discarded rather than re-filed. |
| `CSE370_practice_ch13_query_optimization.pdf` | Ch 13: Query Optimization (Topic 22) — obtained 2026-09-03. **Updated 2026-09-13** per official Final Notice ("new problems added to Lecture 12's practice sheet") — existing Theory/Algebraic Q1–2/Cost-based/Critical-Reasoning content unchanged (verified against `CSE370_t22_query_processing_optimization.md` citations), additions: a new 11-question Theory Questions section, and a third full worked algebraic-optimization example (Hospital DB: Patient/PatientTreatment/Treatment, same 5-step cascade→push-down→reorder→convert-to-⋈→inject-projection procedure as the Aquarius example). |
| `CSE370_practice_ps10_concurrency_control.pdf` | **New, obtained 2026-09-13** per official Final Notice ("short practice sheet on Concurrency Control has been released"). Topic 24 (Lecture 10 all slides) — 2PL growing/shrinking/lock-point/upgrade-downgrade (graph-reading question), deadlock necessary conditions, Wait-Die/Wound-Wait mechanics and application. No separate text version needed (clean text PDF, read directly). |

## Topic-by-Topic Resources

### Foundations (Topics 1–2)
- Silberschatz Ch 1 (Introduction) and Ch 2 (Introduction to the Relational Model)
- Key terms to nail early: relation, tuple, attribute, domain, superkey, candidate key, primary key, foreign key

### SQL (Topics 3–9)
- Silberschatz Ch 3 (Introduction to SQL) and Ch 4 (Intermediate SQL)
- **Video:** CS50's Introduction to Databases with SQL (free, cs50.harvard.edu) — very clear, project-based
- **Video:** freeCodeCamp MySQL full course (YouTube) — 3-hour practical walkthrough
- SQLZoo tutorials: SELECT, SELECT from World, SELECT within SELECT, SUM and COUNT, JOIN, More JOIN
- Practice all queries in XAMPP shell — reading is not enough for SQL

### Relational Algebra (Topic 10)
- Silberschatz Ch 2 (Formal Relational Query Languages section)
- Relational algebra is the theoretical underpinning of SQL — each SQL clause maps to one or more RA operators. Understanding this helps write better queries.

### ER Modeling (Topics 11–13)
- Silberschatz Ch 6 (Database Design Using the E-R Model)
- Draw every ER diagram by hand first, then check notation
- Common exam question: given a scenario, draw the ER diagram and convert to relational schema
- Practice Chen notation (boxes, diamonds, ovals) — BRACU typically uses this

### Normalization (Topics 14–18)
- Silberschatz Ch 7 (Relational Database Design)
- **Recommended supplement:** "A Simple Guide to Five Normal Forms in Relational Database Theory" — William Kent (free online PDF, 1983, still the clearest explanation)
- Work through minimal cover computation by hand until mechanical
- Normalization is consistently the hardest topic for exam performance — allocate extra time

### Storage & Indexing (Topics 19–21)
- **Corrected 2026-08-13** (previous entry cited Ch 13/14, wrong for the 6th-ed. PDF actually in use): Silberschatz 6e **Ch 10 (Storage and File Structure)** and **Ch 11 (Indexing and Hashing)**. Elmasri & Navathe 7e: **Ch 16 (Disk Storage, Basic File Structures, Hashing)** and **Ch 17 (Indexing Structures for Files)**.
- Focus: why B+ trees are preferred over binary search trees and hash indexes for database use
- See `courses/CSE370/concepts/CSE370_t19_indexing.md` for the full concept note (Topics 20–21, built from slides + practice sheet).

### Query Processing (Topic 22)
- **Corrected 2026-09-03** (previous entry cited Ch 15, wrong — verified against the actual Silberschatz 6e table of contents): **Ch 12 (Query Processing)** and **Ch 13 (Query Optimization)**.
- Conceptual understanding sufficient at this level — cost formulas may appear in exams

### Transactions and Concurrency (Topics 23–24)
- **Corrected 2026-09-03** (previous entry cited Ch 17/18, wrong — verified against the actual Silberschatz 6e table of contents): **Ch 14 (Transactions)** and **Ch 15 (Concurrency Control)**.
- The 2PL protocol and ACID properties are the most exam-relevant points here

## Exam Notes
- SQL exams at BRACU are write-the-query format: given a schema and a description, write the SQL.
- ER diagram design and normalization proofs are the other major question types.
- Memorize: SELECT syntax order (SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY). Execution order is different (FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY).
- Normalization: always identify the primary key and all FDs before attempting any NF check.
