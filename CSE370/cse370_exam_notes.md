---
course: CSE370
type: exam-notes
status: active
---

# CSE370 — Exam Notes

## Assessment Creator Map

> [!confirmed] Assessment creator distinction confirmed 2026-06-27.
> - **Centrally created (BRACU question committee):** Midterm, Final, Lab (all except quiz)
> - **Faculty written (NFF / lab faculty):** Quizzes, Assignments, Lab quiz
>
> Do not attribute midterm, final, or non-quiz lab assessment pattern notes to NFF style — they are drawn from a separate central pool. Quiz, assignment, and lab quiz patterns are NFF/lab-faculty-specific.

| Assessment | Created By | Confirmed |
|---|---|---|
| Midterm | Central (BRACU) | Yes |
| Final | Central (BRACU) | Yes |
| Quizzes | NFF | Yes |
| Assignments | NFF | Yes |
| Lab — Quiz | NFF / lab faculty | Yes |
| Lab — Other (mid/final/viva/report) | Central (BRACU) | Yes |

---

## Midterm Pattern

**Syllabus (confirmed via official central announcement, 2026-07-24) — all slides for each lecture are examinable:**

| Lecture | Chapter(s) | Topic |
|---|---|---|
| 1 | Ch 1–2 | Introduction |
| 2 | Ch 3 | ER Model |
| 3 | Ch 4 | EER Model |
| 4 | Ch 5 | Relational Data Model & Database Constraints |
| 5 | Ch 7/9 | Schema Mapping |
| 6 | (updated slides) | Normalization |
| 7 | Ch 8 | Relational Algebra |

**Scope note:** Confirms and completes the earlier classmate report (2026-07-22) — Relational Algebra (Lecture 7) is indeed the last topic in scope. It also **corrects** an earlier assumption: Schema Mapping and Normalization had been filed under the Final's syllabus (Weeks 7–12) below — they are actually **Midterm** content (Lectures 5–6). See the correction note under Final Pattern.

**Exam date:** Wednesday, 2026-07-29
**Time:** 4:30 PM start. **Duration: 1 hour base, extendable ~20–30 min depending on the question set** — reconciles with the 4:30–6:00 PM room-booking window from the official mid-term schedule (2026-07-24); treat 6:00 PM as the ceiling, not a guaranteed fixed length.
**Room:** 08A-02C, Section 11 (official mid-term schedule, 2026-07-24) — resolves the announcement's "verify room on the BRACU website" instruction.
**Arrival:** at least 15 minutes before start (~4:15 PM). No extra time given for late arrivals.
**Weight:** 25% (reconfirmed 2026-08-25)
**Score:** 16.875/25 (67.5%) — reconfirmed 2026-09-13 via CSE370 marks Google Sheet

> [!confirmed] Weight conflict resolved 2026-08-01 (retroactively, via missed official announcement, posted 2026-07-24): exam was out of 40 marks, converted to 25. Notebook's 25% was correct; the earlier 20% (PDF-sourced) was wrong.

**Question pattern (from official announcement, 2026-07-24 — missed until 2026-08-01, exam already sat):**
5 questions, all mandatory. At least: 1 theory/conceptual, 1 ER/EER diagram, 1 Schema Mapping, 1 Normalization problem, 1 Relational Algebra. Marks: 40 raw → converted to 25.

> [!tip] "Explain" questions only need 1–2 lines of justification — do not over-write these.

> [!note] Logged after the fact for future central-exam pattern reference (Final likely follows the same 5-question central-pool structure, though topics differ — unconfirmed until Final's own announcement). Not actionable for this midterm.

**Created by:** Central (BRACU)
**PYQ location:** None. **Confirmed 2026-09-05: not available** — some BRACU courses/sections don't share previous question papers, and this is one of them. Not an open action item; do not keep asking NFF or seniors.

> [!note] No CSE370 PYQs exist. Final (and Midterm, retroactively) revision prep for this course runs on slides + textbook + practice sheets only — no PYQ-based pattern/probable-questions analysis is possible here, unlike CSE321/CSE331.

**Practice sheets (buX, per official announcement 2026-07-24):** `HDD: Course-Library/cse/cse370/practice/` — checked 2026-07-25.

| Lecture | Topic | Status |
|---|---|---|
| 1 | Ch 1–2, Introduction | `CSE370_practice_ch01_intro_db.pdf` present — Ch 2 not separately covered |
| 2 | Ch 3, ER Model | `CSE370_practice_ch03_er.pdf` present |
| 3 | Ch 4, EER Model | `CSE370_practice_ch04_eer.pdf` present |
| 4 | Ch 5, Relational Data Model & Constraints | `CSE370_practice_ch05.pdf` present |
| 5 | Ch 7/9, Schema Mapping | `CSE370_practice_ch07_relational_schema.pdf` present — covers Ch 7 only, no Ch 9 sheet found |
| 6 | Normalization (updated slides) | `CSE370_practice_ch10_normalization.docx` present |
| 7 | Ch 8, Relational Algebra | `CSE370_practice_ch08_relational_algebra.pdf` present — obtained 2026-07-29 |

> [!missing] Ch 9 (second half of Schema Mapping) still has no dedicated sheet — verify whether buX has one or whether Ch 7's sheet is meant to cover both.

> [!tip] Relational Algebra grading risk (per classmate, 2026-07-22): these questions are reportedly easy to score full marks on, but grading is strict from the very first step of the expression — an early mistake can zero the entire answer with no partial credit. Build the expression carefully from the first operator and verify incrementally rather than writing the whole thing in one pass.

<!-- Add pattern notes after PYQs are collected -->

---

## Final Pattern

> [!confirmed] **Official central Final Exam Notice received 2026-09-13** (BUX/Discord announcement, mentioning @everyone). Supersedes the prior user-confirmed working scope (2026-09-06) — that scope is now officially verified correct, with one exclusion refinement noted below.

**Official syllabus (2026-09-13 announcement):**

| Lecture | Coverage | Maps to |
|---|---|---|
| Lecture 8 (Indexing) | **UPDATED — All Slides** | Topics 19–21 (Storage, Indexing, B+ Trees) — `CSE370_t19_indexing.md` |
| Lecture 9 (Database Transactions) | **Pages 1–28 only** (not all 30) | Topic 23 (Transactions) — `CSE370_t23_transactions.md` |
| Lecture 10 (Concurrency Control) | All Slides | Topic 24 — `CSE370_t24_concurrency_control.md` |
| Lecture 11 (Query Processing) | All Slides | Topic 22 part 1 — `CSE370_t22_query_processing_optimization.md` |
| Lecture 12 (Query Optimization) | All Slides | Topic 22 part 2 — `CSE370_t22_query_processing_optimization.md` |

> [!warning] **Lecture 9's "Pages 1–28" cap excludes MV2PL — verified by direct page inspection, 2026-09-13.** The 30-page deck's last two pages: p.29 = "Multiversion Two-Phase Locking Using Certify Locks (MV2PL)", p.30 = closing transition slide to Lecture 10. Pages 1–28 cover ACID, transaction states, log-based recovery (undo/redo), all 5 concurrency problems, SQL isolation levels, and the Multiversion **Timestamp-Ordering** technique (including its worked example) — all still in scope. **MV2PL specifically is now confirmed OUT of scope.** `CSE370_t24_concurrency_control.md` currently documents MV2PL as in-scope material and needs a scope-exclusion flag added.

**SQL (Topics 3–9) remains lab-only; 4NF/5NF (Topic 18) remains out of scope entirely** — both confirmations from 2026-09-06 stand.

**Exam date:** 2026-09-16 (Wednesday), 4:30 PM start. **Duration: 90–100 minutes** (be present ≥15 min before start).
**Questions:** 5, all mandatory — confirms the note logged under Midterm Pattern above speculating the Final would follow the same 5-question central-pool structure.
**Marks:** Exam out of 40–50 raw marks, converted to 30 (same raw→weighted conversion pattern as Midterm's 40→25).

> [!confirmed] **Question-weight breakdown confirmed 2026-09-17** from the "CSE370 Section 11 ALL" marks Google Sheet's Final tab structure: Q1(10), Q2(8), Q3(12), Q4(5), Q5(10) — raw total 45, scaled to 30. **Marks not yet released** as of this check — the sheet's Final tab is still all-zero for the whole section, not just this student. Archived (sat, pending) to `tracking/tasks-history.md` #51; revisit once the sheet publishes a result.
**Weight:** 30% *(resolved 2026-08-01 by arithmetic: Assignment 5% + Quiz 10% + Midterm 25% (confirmed) + Project 15% + Lab work 15% = 70%, leaving 30% for Final — matches notebook, not the outline PDF's 35%)*
**Created by:** Central (BRACU)
**Room:** check via https://www.bracu.ac.bd/final-exam-schedule-summer-2026-except-phr

**New/updated resources per this announcement, not yet fetched (2026-09-13):**
- A new, short practice sheet on Concurrency Control has been released (BUX) — not yet on the HDD.
- Lecture 9's practice sheet (`ps9_transactions.pdf`) and Lecture 12's practice sheet (`ch13_query_optimization.pdf`) have both had new problems added on BUX — HDD copies are now stale and should be re-downloaded before final practice.

**Standard exam-conduct notes from the announcement:** no questions during the exam (understanding the question is part of the exam), no washroom breaks, no unfair means (expulsion risk), ID card mandatory.

> [!confirmed] **Query Tree Optimization (Algebraic Modifications) — official procedure, per Central Discord announcement ("370 central"), received 2026-09-16 (day of exam).** When a question asks to optimize a query tree via algebraic modifications, the expected process is 5 steps, with a specific drawing requirement:
> 1. Cascade of Selection (σ)
> 2. Pushdown selection to relevant leaf
> 3. Apply the most restrictive selection first (compare operators, or check the catalog)
> 4. Convert Cartesian Product (✖) to Theta Join (⋈)
> 5. Inject Projections (π) where applicable
>
> **Drawing requirement:** Steps 1–2 may be combined into one query tree. Step 3 **must** be shown as a separate, distinct query tree. Steps 4–5 may be combined into one final query tree. **Minimum 3 query trees** total (or all 5 shown individually) — from the initial tree to the fully optimized tree.
>
> Governs Lecture 12 (Query Optimization) → directly applies to Slot 5(a) ("Heuristic Algebraic Rewrite") in `cse370_final_mock_exam.md`. Any grading-by-step-count risk (à la Relational Algebra's strict-from-first-step grading, Midterm Pattern above) is unconfirmed for this question type — treat the tree-count requirement as a hard floor regardless.

<!-- Add pattern notes here -->

---

## Quiz Pattern

**Weight:** 10% (4 quizzes, best 3 averaged)
**Created by:** NFF

> [!confirmed] **Component finalized: 9/10 — confirmed 2026-09-17 via the "CSE370 Section 11 ALL" marks Google Sheet.** All four counted slots now graded: Quiz 1 = 7/10, Quiz 2 Makeup = 10/10 (replaces the missed Quiz 2), Quiz 3 = 10/10, Quiz 4 = 7/10. Best 3 of 4 drops one of the two 7s → average of {10, 10, 7} = **9/10**, no components still pending.

### Quiz 2

**Date:** 2026-07-20 (Monday), during class — postponed from 2026-07-15 by NFF on 2026-07-15
**Syllabus:** EER model, database constraints, schema mapping (ERD and EERD → relational schema)
**Announced:** 2026-07-12 by NFF
**Status:** Missed (illness, 2026-07-20) — makeup quiz confirmed by NFF via email 2026-07-21, date TBC (end of semester), replaces Quiz 2 directly in the "best 3 of 4" average. See `tracking/tasks-active.md` #8.

### Quiz 3

**Date:** 2026-08-19 (Wednesday), during class
**Syllabus:** Indexing
**Announced:** 2026-08-15 by NFF (official announcement)
**Status:** Sat. Marks pending. Archived to `tracking/tasks-history.md` #31.

**Question pattern (from this sitting):** 2 questions, 10 marks total.
- **Q1 (8 marks):** B+ tree insertion — order (n) 3 specified, 8 given data values inserted one at a time; student draws the tree state after each insertion, including splits.
- **Q2 (2 marks):** Index search — given two search keys, demonstrate the lookup/traversal steps on the same tree built in Q1 (root → internal → leaf, following the ordering invariant).

**Self-assessment:** Both questions attempted. Self-assessed "probably made some misses" — specific error location not pinned down (exact insertion sequence and search paths were not reconstructed in the post-quiz debrief).

**Score: 10/10 — confirmed 2026-09-17 via the "CSE370 Section 11 ALL" marks Google Sheet.** Full marks — the official result beats the self-assessment outright, same pattern as Quiz 4. Archived to `tracking/tasks-history.md` #31.

### Quiz 4

**Date:** 2026-09-07 (Monday) — **CONFIRMED** via official NFF Discord announcement, 2026-09-03.
**Time:** 9:30 AM (regular class slot) — not separately announced as exam-specific in the Discord announcement.
**Syllabus:** Transactions, Concurrency Control, Locks (confirmed) — matches Lecture 9 (Transactions) and Lecture 10 (Concurrency Control), consistent with the earlier verbal "lecture slides 9 and 10" estimate.
**Announced:** In-class verbal, 2026-08-31 (unconfirmed at the time) — officially confirmed via Discord, 2026-09-03.

> [!note] NFF stated in-class (verbal, unconfirmed status per `feedback_verbal_declaration_not_confirmation`) that the quiz will be short and easy, reasoning that some syllabus (likely Concurrency Control, Lecture 10) is still being covered. User assesses this as plausible given the remaining-coverage constraint. Treat as an expectation signal, not a confirmed question-pattern or difficulty guarantee.

**Also announced 2026-08-31 (in-class), same unconfirmed status at the time:** a makeup quiz to be held on the day of the Final Exam (2026-09-16 — see Final Pattern above, and the Quiz 2 Makeup section below). User-confirmed this is the same makeup already tracked as `tracking/tasks-active.md` task [7] (Quiz 2 makeup), not a separate slot.

**Status:** Sat. Self-assessed quite bad overall. **Score: 7/10 — confirmed 2026-09-13 via CSE370 marks Google Sheet** (higher than the self-assessment going in). Archived to `tracking/tasks-history.md` #43.

**Question pattern (from this sitting):** one concurrency scenario (2 transactions interacting concurrently), 5 small sub-questions built around it — matches the Teller/Auditor worked example already in `concepts/CSE370_t24_concurrency_control.md` (sourced from `CSE370_practice_ps9_transactions.pdf` Q14) almost exactly, and was the top-ranked prediction that session's `CSE370_quiz4_probable_questions.md` made (file since deleted per its own temporary-file note, now that the quiz has sat).

- **Q1:** Identify the concurrency problem in the scenario → **Dirty Read**. Missed live during the quiz; confirmed correct by NFF post-quiz.
- **Q2:** Name the ACID property violated → **Isolation**.
- **Q3:** How could this have been prevented → **Serializable isolation level, or Strict 2PL** (hold the X-lock until the writer commits, so the reader's request blocks instead of seeing uncommitted data).
- **Q4:** Content not recalled post-quiz. Best guess, unconfirmed: given the scenario's writer transaction rolled back, a recovery-technique question (what log record/technique is needed — an **undo log**, since no commit record exists for that transaction).
- **Q5 (outside the scenario):** Distinguish the two phases of Two-Phase Locking → **Growing** (acquire locks only, no release) vs. **Shrinking** (release only, no new acquisition/upgrade), separated by the **lock point** (the moment of the final lock).

**Self-assessment:** Failed to identify Q1 live under quiz conditions — the specific gap was live scenario-to-named-problem recognition under time pressure, not conceptual understanding: Q2, Q3, and Q5 were all correctly reconstructed without difficulty in a same-day post-quiz session once talked through. Full pattern-analysis detail in `tracking/tasks-history.md` #43.

### Quiz 2 — Makeup

**Date:** 2026-09-16, after the Final Exam, same day — **CONFIRMED** via official NFF Discord announcement, 2026-09-03 (date/timing already matched the 2026-08-31 verbal estimate; this announcement makes it official).
**Syllabus:** **"Final Syllabus"** (per the 2026-09-03 official announcement) — this **supersedes** the earlier assumption that the makeup would cover the same topic as the original Quiz 2 (EER model, database constraints, schema mapping). Treat the makeup as covering the Final's full syllabus, not the original Quiz 2 topic, until/unless NFF clarifies further.
**Sign-up requirement:** Students who want to sit for the makeup must register on a Google Sheet (link shared in the 2026-09-03 announcement) — done, 2026-09-03.
**Announced:** In-class verbal, 2026-08-31 (unconfirmed at the time) — officially confirmed via Discord, 2026-09-03.

**Status:** Sat 2026-09-16. **Score: 10/10 — confirmed 2026-09-17 via the "CSE370 Section 11 ALL" marks Google Sheet.** Full marks, replacing the missed original Quiz 2 (0, illness) directly in the best-3-of-4 average. Archived to `tracking/tasks-history.md` #50.

<!-- Add pattern notes after first quiz -->

---

## Assignment Pattern

**Weight:** 5% (3 assignments)
**Created by:** NFF

**Component score (confirmed 2026-09-13 via CSE370 marks Google Sheet): 3/5.** Known inputs: Assignment 1 = 5/5 full marks (confirmed 2026-08-25); Assignment 2 = 0/5, missed (`tracking/tasks-history.md` #19). Assignment 3's individual score is not separately confirmed anywhere — back-solving from the other two under a straight-average assumption implies Assignment 3 ≈ 4/5-equivalent, but this is an unconfirmed inference, not an official figure. Do not cite an Assignment 3-specific score without a direct confirmation.

### Assignment 3

**Released:** 2026-09-03 (Discord) | **Deadline:** 2026-09-09 (Wed)
**Format:** Handwritten, Google Form submission, no late submission accepted (confirmed pattern, matches Assignments 1–2).
**Content:** Hash index construction (forward chaining), B+ Tree sequential deletion (order 3) — full detail: `courses/CSE370/assignments/CSE370_assignment_03.md`.

<!-- Add pattern notes here. Ask about submission format for each assignment. -->

---

## Lab Assessment Pattern

**Weight:** 30% (lab component) — 40 raw marks converted to 30
**Created by:** Central (BRACU) for all lab assessments except lab quiz (NFF / lab faculty)

### Confirmed from Lab Intro Slide (2026-06-27)

| Component | Marks | Type | Notes |
|---|---|---|---|
| Assignment | 5 | Individual | Average of all 3 |
| Lab Quiz | 15 | Individual | Average of 2 quizzes — Week 4 and Week 7 |
| Project Report | 5 | Group | Details TBD |
| Group Work | 3 | Group | Contribution, cohesion, report format |
| Project Simulation | 6 | Individual | 3 features per student, 2 marks each |
| Project Viva | 6 | Individual | Viva on own features — SQL queries, data storage/fetch/modify |

### Lab Quiz Pattern

**Quizzes:** 2 total. Week 4 and Week 7.
**Topic focus:** SQL — CRUD, WHERE filtering, aggregation, subqueries, GROUP BY, HAVING, JOINs, constraints.
**Created by:** Lab faculty (TMD / NAHC).

<!-- Add specific question pattern after Quiz 1 (Week 4) -->

### Project Viva Pattern

**Week:** 10. **Individual evaluation per member.**
**Focus:** Your 3 features — SQL queries behind them, how data is stored, fetched, modified.
No group-level viva — each person is evaluated on their own contribution.

<!-- Add specific viva question pattern after first session -->
