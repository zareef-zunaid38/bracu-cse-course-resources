---
course: CSE321
type: exam-notes
status: active
---

# CSE321 — Exam Notes

## Assessment Creator Map

> [!confirmed] Assessment creator distinction confirmed 2026-06-27.
> - **Centrally created (BRACU question committee):** Midterm, Final, Assignments, Lab (all except quiz)
> - **Faculty written (NTR / MAFZ / SZD):** Theory quiz (NTR), Lab quiz (MAFZ / SZD)
>
> Do not attribute midterm, final, assignment, or non-quiz lab pattern notes to NTR or MAFZ style — they are drawn from a separate central pool. Quiz and lab quiz patterns are faculty-specific.

| Assessment | Created By | Confirmed |
|---|---|---|
| Midterm | Central (BRACU) | Yes |
| Final | Central (BRACU) | Yes |
| Quizzes | NTR | Yes |
| Assignments | Central (BRACU) | Yes |
| Lab — Quiz | MAFZ / SZD | Yes |
| Lab — Other (mid/final/viva/report) | Central (BRACU) | Yes |

---

## Midterm Pattern

**Syllabus (confirmed via official central announcement, 2026-07-24):**

| Topic | Question types |
|---|---|
| Process | Conceptual questions; code simulations |
| Threads | Conceptual questions; code simulations |
| CPU Scheduling | Conceptual questions; algorithm simulations and mathematics from scheduling algorithms |
| Process Synchronization | Conceptual questions; code simulations |

**Exam date:** 2026-07-30, 2:00–3:30 PM, Room 07B-13C (Sec 2) — confirmed via official mid-term schedule 2026-07-24
**Weight:** 25%
**Created by:** Central (BRACU)
**PYQ location:** `CSE321/pyq/` — multiple semesters available, see [pyq_index](pyq/pyq_index.md)

<!-- Add pattern notes after reviewing PYQs -->

---

## Final Pattern

**Syllabus (confirmed via official central announcement, 2026-09-06):**

| Topic | Question types |
|---|---|
| File Systems | Conceptual questions; Mathematics & simulations |
| Memory Management | Conceptual questions; Mathematics & simulations |
| Protection | Conceptual questions |
| Security | Conceptual questions |

Note: Protection and Security carry conceptual questions only per the confirmed announcement — no math/simulation component for those two, unlike File Systems and Memory Management. Broader topic span (Weeks 9–14) unchanged from prior note.

**Exam date:** 2026-09-17, 2:00–4:00 PM
**Weight:** 35%
**Created by:** Central (BRACU)
**PYQ location:** `CSE321/pyq/` — multiple semesters available, see [pyq_index](pyq/pyq_index.md)

> [!note] Grading, per NTR (stated in-class 2026-08-24): even though the Final is centrally created, NTR grades her own section's scripts to the central rubric's numbers, sometimes showing leniency. So central creation sets the questions, not necessarily the mark-by-mark strictness — worth keeping in mind for borderline answers rather than assuming zero-tolerance rubric adherence.

> [!confirmed] **Final result: 24/35 (68.6%) — from NTR's posted final marks, 2026-09-20.** The faculty shared only the final mark and grade; no per-question breakdown exists. **Course total: 65.25/100 → C+ (2.30)** — Lab 21.25/25 + Assignments 5/5 + Quizzes 5.00/10 + Midterm 10/25 + Final 24/35. **Grade confirmed:** NTR's posted final grade shows C+, matching the computation and confirming the 65 band edge. Archived to `task-history/tasks-history.md` #45.

<!-- Add pattern notes here -->

---

## Quiz Pattern

**Weight:** 10% (4 quizzes, best 3 averaged)
**Created by:** NTR

> [!note] Grading policy (NTR, stated in-class 2026-08-24): no partial marks on quiz questions — full marks per question or none.

> [!note] Quiz difficulty ranking, NTR's own stated ranking (in-class, 2026-08-31): hardest to easiest — **Quiz 3 > Quiz 1 > Quiz 4 > Quiz 2**. **Cross-checked against official results, all four quizzes now graded (confirmed 2026-09-06):** Quiz 1 = 2.5/10 (first recorded as 2/10, see the Makeup Quiz callout below), Quiz 2 = 0/10 (missed), Quiz 3 = 6/10, Quiz 4 = 1/10 (`task-history/tasks-history.md` #5, #17, #27, #36). The ranking does not hold against actual scores — Quiz 3 (stated hardest) scored *highest*, and Quiz 4 (stated easier than Quiz 1 and Quiz 3) scored *lowest* among sat quizzes. Read with caution: score is confounded by prep depth (Quiz 4 was a near-cold-start attempt, `tasks-history.md` #36) as much as by question difficulty, so this doesn't necessarily disprove NTR's ranking — it shows self-reported prep effort dominated the outcome more than intrinsic quiz difficulty did this cycle.

> [!confirmed] **Makeup Quiz officially graded: 6.5/10 — confirmed 2026-09-17 via "321 marks.png.pdf" gradesheet screenshot.** The makeup quiz (Virtual Memory, sat 2026-09-08) replaces the missed Quiz 2 (0/10, `task-history/tasks-history.md` #17) in the best-3-of-4 pool, per standing course policy that a makeup supersedes the slot it was sat to cover — not an additional 5th score. **Best-3-of-4 pool is now: Quiz 1 = 2.5/10, Makeup (replacing Quiz 2) = 6.5/10, Quiz 3 = 6/10, Quiz 4 = 1/10 → drop Quiz 4 (lowest) → average of {2.5, 6.5, 6} = 5.00/10.** **Quiz 1 discrepancy resolved 2026-09-20:** the same screenshot shows Quiz 1 as 2.5/10, and the 2/10 first recorded on 2026-08-09 was the integer version of the same mark (per the user) — 2.5/10 is the figure of record. Midterm (10/25) and Quiz 3/4 (6/10, 1/10) on the same sheet match already-confirmed figures exactly, supporting the sheet's reliability overall.

**Chapter 8 quiz (announced 2026-08-24, verbal only):** date given as 2026-08-31; later officially confirmed as Quiz 4, sat 2026-09-02, 1/10 (see `task-history/tasks-history.md` #36). "Chapter 8" = Memory Mgmt, Topics 24–26 in `cse321_topics.md` (memory basics/address spaces, contiguous allocation/fragmentation, paging) — user-confirmed 2026-08-24. Does not include Virtual Memory (Topic 27) or page replacement (Topic 28); same announcement flagged a probable makeup quiz on Virtual Memory covering that remainder, no date given at the time — see the Virtual Memory makeup entry directly below for the date NTR has since given.

**Virtual Memory makeup quiz — sat 2026-09-08.** Date given in-class 2026-08-31 (verbal only), confirmed via official NTR email 2026-09-03 (a verbal statement, not counted as confirmed until officially announced), location corrected 2026-09-06 to Room 10B13C. See the pattern entry below for question breakdown.

---

### Quiz 1 — 2026-07-01 | Chapter Process | Total: ~10 marks

**Format:** Mix of analytical (code trace) and theoretical (short written answer) and scenario-based.

| # | Question | Marks | Topic |
|---|---|---|---|
| Q1 | Nested fork() code given — show the output | 3 | fork() tracing |
| Q2a | Process Control Block — write about it | 3 | PCB |
| Q2b | Relation between process states and scheduler | 2 | States + schedulers |
| Q3 | IPC scenario — user wants shared storage, large capacity, speed not a priority. Which IPC method? | 2 | IPC — Shared Memory vs Message Passing |

**Q3 correct answer:** Shared Memory. The scenario describes a need for large data capacity ("larger storage") with no speed constraint. Shared memory maps a region directly into both processes' address spaces — it provides the "shared storage" the scenario describes. Message passing does not provide persistent storage; it transfers discrete messages. Speed not being a concern does not eliminate shared memory — it simply removes the one advantage that would favour it over message passing.

**Observed tendencies (NTR, Quiz 1):**
- One analytical code-tracing question (fork()) — worth the most marks
- Theoretical questions are short written answers, not MCQ
- IPC appeared as a scenario question, not a direct compare-and-contrast
- PCB asked as a standalone definition/explanation question
- No pipe or socket question appeared
- No state diagram draw question appeared (contrary to prediction)

**Result:** 2.5/10 (official gradesheet, 2026-09-17 screenshot; first recorded as 2/10 on 2026-08-09, the integer version of the same mark). User reported insufficient preparation.

---

### Quiz 3 — 2026-08-19 | File System (Lecture 5) | Total: 10 marks

**Format:** Mix of calculation (sector address, double-indirect block count) and procedural (system call steps) and scenario-based (journaling method choice).

| # | Question | Marks | Topic |
|---|---|---|---|
| Q1 | Find the sector address of an inode (details given) | 3 | Inode Byte/Sector Address Calculation (Topic 22, Key Skill 2) |
| Q2 | Illustrate the steps of `open()` and `write()` system calls (file location given) | 3 | File Operations (Topic 20) |
| Q3 | Find the number of double indirect blocks needed (info given, max storage given) | 2 | Indexed Allocation / UNIX Inode — reverse capacity calc (Topic 22) |
| Q4 | Scenario given — decide which of two journaling methods to use | 2 | Crash Consistency / Journaling (Topic 23) |

**Self-assessed result:** Did well on Q1 (sector address calc) and Q4 (journaling scenario) — the first and last questions. Did *not* do well on the middle two: Q2 (`open()`/`write()` steps) and Q3 (double-indirect block count). Overall self-rating: "did not do nor good nor bad."

**Observed tendencies (NTR, Quiz 3):**
- Confirms "Lecture 5" as a quiz topic spans the *entire* Module 5 (Topics 20–23), not a narrower slice — this quiz drew from File Operations (20), Indexed Allocation/Inode (22, two separate questions — forward *and* reverse capacity calculation), and Crash Consistency/Journaling (23)
- No question drawn from File System Structure / Ext2 layout (Topic 21) this time
- Result does *not* split cleanly along "calculation vs. conceptual" — Q1 (numeric, forward-direction sector-address calc) went well, but Q3 (numeric, *reverse*-direction indirect-block calc) did not; Q4 (scenario/decide) went well, but Q2 (procedural "illustrate the steps of open()/write()") did not. The cleaner read: strong on the calculation format already drilled repeatedly in revision (Key Skill 2, forward direction), weak on two formats not previously drilled in this depth — the step-by-step system-call walkthrough (Q2) and the *inverse* capacity calculation (Q3)
- First quiz to test the inverse direction of a Topic 22 calculation (given capacity/storage, find required indirect-block count) rather than only the forward direction (given structure, find max size) — the miss here specifically flags this as an under-practiced format to add to Final prep, alongside walking through `open()`/`write()` step-by-step in writing rather than just knowing what each call does

**Result:** 6/10 (official gradesheet, confirmed 2026-09-06).

---

### Makeup Quiz — 2026-09-08 | Main Memory and Virtual Memory (Topics 24–28) | Total: 10 marks

**Format:** One simulation/comparison question, one multi-part address-translation question with a scenario dressing.

| # | Question | Marks | Topic |
|---|---|---|---|
| Q1 | Reference-string page-replacement simulation, 3 frames — compare **Optimal vs. LRU** (no FIFO) | 7 | Page Replacement Algorithms (Topic 28) |
| Q2a | Given a list of logical addresses, a page-number→frame-number table, and a page-number→filename table (one file per page) — compute the physical address for each, or report invalid if the page isn't present | 2 | Address Translation (Topic 26) |
| Q2b | Describe how an 8-frame RAM would "look" to the user — i.e. which frames are occupied by which file | 1 | Address Translation / user-view of memory (Topic 26) |

**Self-assessed result:** Did quite good overall on both questions; did not have time to double-check work before submitting.

**Result:** 6.5/10 — confirmed 2026-09-17 via "321 marks.png.pdf" gradesheet screenshot. Matches the "did quite good" self-assessment. Replaces the missed Quiz 2 (0/10) in the best-3-of-4 pool — see the confirmed callout under Quiz Pattern above.

**Observed tendencies (NTR, Makeup Quiz):**
- Page-replacement comparison narrowed to Optimal vs. LRU only this time — FIFO excluded, a change from the standard three-way comparison drilled in revision.
- The address-translation question was dressed up with a "file per page" framing rather than asking for a bare page-number→frame-number lookup — same underlying Topic 26 math, unfamiliar packaging. This is the exact question type that came up as an unreconstructed follow-up earlier in prep (see this session's teaching thread) — now resolved: it's a standard translation lookup, plus a second sub-part asking for a **user-facing view of RAM occupancy** (which frames hold which file), not just raw addresses.
- Reinforces the standing no-partial-marks policy (above) as the main source of risk on a self-assessed "did well" outcome — a single slip on either question could zero it out regardless of overall understanding.

---

## Assignment Pattern

**Weight:** 5% (2 assignments)
**Created by:** Central (BRACU)

**Scores (confirmed 2026-09-17 via "321 marks.png.pdf" gradesheet screenshot):** Assignment 1 (Threads comparison) = **5/5**. Assignment 2 (huge/superpages research report) = **5/5**. Both full marks — first numeric scores recorded for either; previously only "Submitted" with no result.

<!-- Add pattern notes here. Ask about submission format for each assignment. -->

---

## Lab Assessment Pattern

**Weight:** 7% of lab grade per assessment slot (average of 2 assessments, out of 25 total lab marks — see `courses/CSE321/lab/lab_index.md`)
**Created by:** Central (BRACU) — see Assessment Creator Map above ("Lab — Other")

---

### Assessment 1 — 2026-07-07 | Basic C Programming and Bash Commands | 7 marks

**Format:** Two coding tasks — one shell/bash task, one C task.

| # | Task | Description |
|---|---|---|
| Shell | 4-command sequence | `mkdir` → `cd` → `touch` → write → append → display final output |
| C | Array sum split | Read n values into an array, sum even/odd separately via a user-defined function, compute the difference, compile via bash |

**Result:** 5.5/7 (`task-history/tasks-history.md` #8). Shell: lost 1 mark for omitting `cat` to display the final output — the write/append steps themselves were correct. C: code had a bug so it didn't execute correctly, but viva performance recovered most of the lost marks (net loss only 0.5).

---

### Assessment 2 — 2026-08-25, 9:20 AM sharp | System Calls, Threads, Process Synchronization | 10 marks raw (converts to the 7-mark slot)

**Format:** Two coding problems, 5 marks each. Question wording below reconstructed from memory after the exam — not verbatim.

| # | Task | Description |
|---|---|---|
| Q1 | Nested `fork()` | Create a child process, then a grandchild from that child. Check a modulo condition across the PIDs (e.g. `(grandchild_pid % grandparent_pid) % 7 == 0`) and print whether it's divisible. |
| Q2 | Two threads, shared function | Create threads A and B from the *same* thread function; thread A performs one set of minor tasks, thread B performs a different set. |

**Result:** 3.5/10 (`task-history/tasks-history.md` #31). Q1 attempted, not close to a working solution (faculty leniency applied anyway). Q2 not attempted.

**Observed tendencies (Assessment 1 vs. 2, both MAFZ/SZD):**
- Both assessments are two-part, roughly-even-weighted coding problems — not a single large problem.
- Assessment 1 tested single-generation process creation + basic shell; Assessment 2 escalated to **multi-generation** process chaining (parent → child → grandchild, not just parent → child) and **multi-thread task differentiation** from one shared function — both meaningfully harder than anything drilled in the lab's own `thread1.c`–`thread6.c` examples, which mostly demonstrate one thread function used identically across threads, not divergent per-thread behavior.
- Precedent from a prior cohort suggested these assessments stay close to "basic code" — held for Assessment 1, did not hold for the nested-fork/divergent-thread format in Assessment 2. Worth re-weighting that signal down for future assessments in this pair, not treating it as reliable.

<!-- Update this section after official marks/leniency details are confirmed for Assessment 2. -->
