---
course: CSE331
type: exam-notes
status: active
---

# CSE331 — Exam Notes

## Assessment Creator Map

> [!confirmed] Assessment creator distinction confirmed 2026-06-27.
> - **Centrally created (BRACU question committee):** Midterm, Final
> - **Faculty written (TNF):** Quizzes, Assignments
>
> Do not attribute midterm or final pattern notes to TNF style — they are drawn from a separate central pool. Quiz and assignment patterns are TNF-specific. `CSE331/pyq/other-faculty-assignments/` contains past assignments from multiple faculty (RSS, KKP, AFA, RFR, FDM) — filter for TNF-authored ones only.

| Assessment | Created By | Confirmed |
|---|---|---|
| Midterm | Central (BRACU) | Yes |
| Final | Central (BRACU) | Yes |
| Quizzes | TNF | Yes |
| Assignments | TNF | Yes |

---

## Midterm Pattern

**Syllabus:** See [cse331_topics](topics/cse331_topics.md)
**Exam date:** 2026-07-28, 4:30–6:00 PM, Room 09D-18C (Sec 16) — confirmed via official mid-term schedule 2026-07-24 (supersedes the "duration TBD" note from TNF's 2026-07-21 announcement)
**Total marks:** 50
**Weight:** 35%
**Created by:** Central (BRACU)
**PYQ location:** `CSE331/pyq/` — check if midterm/final question papers have been added, see [pyq_index](pyq/pyq_index.md)

**Confirmed syllabus (TNF announcement, 2026-07-21):**
- Alphabets, Strings, and Languages
- DFAs and Regular Languages
- The Regular Operations (Union, Concatenation, Kleene star)
- Nondeterminism and NFAs
- Equivalence between NFAs and DFAs (Subset Construction) — **scope confirmed by TNF in class (student Q&A, 2026-07-24 announcement):** only the NFA→DFA conversion direction is examinable.
- Closure under the Regular Operations
- Regular Expressions
- Equivalence between Regular Expressions and Finite Automata
- Converting Regular Expressions to NFAs
- Converting DFAs to Regular Expressions — **method constraint confirmed by TNF (2026-07-26): state elimination is the only accepted method.** State equations / Arden's Theorem will not be accepted, even though it was leniently half-credited once on Quiz 2 (see Quiz Pattern below).

**Proof-of-correctness guidance (TNF, confirmed 2026-07-24 — supersedes the earlier secondhand classmate note):**
- Proof questions may appear on the Mid-Term. Answer with a **generalized case** — proof by a specific example language is explicitly **not accepted**.
- "Writing correct method to the point will be enough" — no need to overly elaborate any part. Full formal notation is not mandatory; explaining the underlying idea is sufficient.
- The topics below are already worked examples in the textbook, so TNF considers them lower-probability as direct Mid-Term questions — but understanding how to write each as a generalized proof is still expected:

| Proof topic | Book page(s) | What to write |
|---|---|---|
| Closure under Union | p.46 (DFA cross product) or p.59 (multiple paths through NFA) — both done in class | No notation required — explain how transitions, start state, and accepting states are constructed (or the multi-path NFA argument) |
| Closure under Concatenation | p.61 | Construction process as done in class |
| Closure under Kleene Star | p.62 | Construction process as done in class |
| NFA → DFA equivalence | p.55 | Union of states reached on the same input, ε-closure, start state, accepting states |
| RE → NFA equivalence (a language with a regular expression is regular) | p.67 | Construction process as done in class |
| FA → RE equivalence (a regular language has a regular expression) | p.70 | DFA → NFA → GNFA → RE, as done in class |

All in-class construction methods are the foundation for any proof question that might appear — understanding the generalized steps for each is enough; complex notation is not required.

One example proof was shown in class before this official guidance (NFA → single accepting state via ε-transitions) — transcribed at `courses/CSE331/practice/CSE331_nfa_single_accepting_state_proof.md`.

> [!confirmed] **Result: 29.05/35 (83%) — confirmed 2026-09-17 via the official "CSE331-16 Summer 2026 Attendance and Marksheet" Google Sheet** (raw 41.5/50, scaled ×35/50). This corrects the 29.5/35 figure previously recorded in `task-history/tasks-history.md` #19 — the two don't match, and the sheet is treated as authoritative since it also carries the final course-grade computation cross-checked against every other component. Cause of the original discrepancy not identified.

<!-- Add pattern notes after locating PYQs -->

---

## Final Pattern

**Confirmed syllabus (TNF official announcement, received 2026-09-11) — supersedes the 5-item announcement of 2026-09-06 (same content, TNF's own re-itemization; not a scope change):**
1. Pumping Lemma for Regular Languages
2. Context-Free Languages (CFL)
3. Context-Free Grammars (CFG)
4. Parse Tree, Derivation, and Ambiguity
5. Pushdown Automata (PDA)
6. Turing Machine (TM)
7. Decidability and The Halting Problem
8. Reducibility

Item 1 now explicitly says "for Regular Languages" — confirms the earlier lecture-note-coverage resolution below (CFL Pumping Lemma was already ruled out; this removes the ambiguity outright rather than just inferring it).

**Scope note (TNF, Quiz 5 announcement):** two-stack PDA is explicitly excluded from the Final syllabus (also excluded from the Quiz 5 syllabus, see Quiz Pattern below) — TNF's own statement despite the Final being centrally created, so treat as a reliable syllabus-scope signal even though she doesn't write the questions. Still applies under item 5 above.

**Scope rule for anything not named in the 8-item list (user directive, 2026-09-06): if a topic was never covered in a lecture note, it is not part of the Final.** Resolved against [cse331_topics](topics/cse331_topics.md) by checking actual lecture-note coverage:
- Topics 1–7 (DFA/NFA/RE/subset construction/closure/minimization) — covered in lecture notes (L1–L9) but absent from the list, which starts at Pumping Lemma — **out of scope.**
- Topic 12, Chomsky Normal Form — **never covered in any lecture note.** L18's `chomsky-hierarchy-diagram` topic is the Chomsky *hierarchy* classification diagram (regular/CFL/CSL/RE), a different concept despite the name overlap; CNF itself (the conversion procedure) appears nowhere — **out of scope.**
- Topics 15–16, Pumping Lemma for CFLs / non-context-free proofs — **never covered, and now explicitly excluded by item 1's own wording ("for Regular Languages")** — **out of scope, confirmed both ways.**
- Topics 18–19, TM Variants and Church-Turing Thesis — **never covered.** No lecture note (L16–L19) mentions multi-tape TMs, nondeterministic-TM equivalence, or the Church-Turing Thesis — item 6 ("Turing Machine") means the base 7-tuple TM definition and construction only — **out of scope.**

**Exam date:** 2026-09-15, 4:30–6:30 PM, confirmed via official room/section schedule — the 2026-09-11 announcement lists "Duration: TBD" but this predates/doesn't override the separately confirmed room/section time slot (see commit `1bfeca0`).
**Total marks:** 50 (matches the 2026-09-11 announcement)
**Weight:** 35%
**Created by:** Central (BRACU)
**PYQ location:** `CSE331/pyq/` — 3 sittings, Sets A+B (Spring25, Fall25, Spring26). Full pattern analysis: [CSE331_final_probable_questions](pyq/CSE331_final_probable_questions.md).

**Reducibility confirmed new this semester (TNF, Discord, 2026-09-11):** asked directly why no past Final PYQ (Spring25/Fall25/Spring26) included a full undecidability/reduction proof — TNF confirmed "Reducibility was not included in earlier syllabus." So its absence from all 3 collected sittings isn't a low-probability signal — it was structurally impossible to test before. It's explicitly in this semester's confirmed syllabus (item 8, separated out from Decidability/Halting Problem at item 7 in the reissued 8-item list) with zero past-paper precedent to practice from. Treat as high-priority, not low-probability, going into 2026-09-15.

**Stable across all 3 sittings collected:** Problem 1 is always a 5-point Pumping Lemma proof (CO5). Derivations/Parse Trees/Ambiguity is always exactly 10 points, same 4-part structure. CFG design and PDA design are always present (15 pts each in the older Spring25 format, 10 pts each in the current Fall25/Spring26 format). Fall25/Spring26 additionally carry a 5-point True/False section absent in Spring25. See [CSE331_final_probable_questions](pyq/CSE331_final_probable_questions.md) for full breakdown, module-by-module question types, and ranked probable questions.

> [!confirmed] **Result: 24.33/35 — confirmed 2026-09-17 via the official CSE331 marks Google Sheet** (raw 34.75/50, scaled ×35/50, no bonus). Per-component breakdown from the sheet's Final tab:
>
> | Component | CO | Marks | Score |
> |---|---|---|---|
> | Pumping Lemma | CO5 | /5 | 0 |
> | CFG | CO3 | /10 | 7 |
> | Parse Tree/Derivation/Ambiguity | CO3 | /10 | 10 |
> | PDA | CO3 | /10 | 9.5 |
> | Non-Regular Languages | CO3 | /5 | 2.25 |
> | Turing Machine | CO4 | /10 | 6 |
> | **Total** | | **/50** | **34.75** |
>
> Pumping Lemma (0/5) is the single largest loss on the paper, despite Reducibility (item 8, zero past-PYQ precedent) having been flagged pre-exam as the highest-priority review item — everything else scored at or above 60%, with Parse Tree a clean 10/10. Full archive entry: `task-history/tasks-history.md` #42.
>
> **This also finalizes the entire CSE331 course grade** (same sheet, Final GradeSheet tab): Quiz 19.17/20 (20%) + Assignment 4.75/10 (10%) + Midterm 29.05/35 (35%) + Final 24.33/35 (35%) = **77.3/100 → 77 → Grade B (3.00), plain suffix**.

---

## Quiz Pattern

**Weight:** 20% (4 quizzes, best 3 averaged)
**Created by:** TNF
**Quiz 1 date:** 2026-07-01, 8:10 AM (postponed from 2026-06-29)

### Quiz 1 — Confirmed Pattern (2026-07-01)

**Syllabus:** DFA construction · Regular Operations in DFA · Cross product method (Regular expressions excluded — faculty announcement)
**Total marks:** 20

| Question | Content | Marks |
|---|---|---|
| Q1 | 2 DFA constructions (large) | 5 + 5 = 10 |
| Q2 | 2 DFA constructions (smaller) + cross product between them | 3 + 3 + 4 = 10 |

**Set A — Actual Questions:**

| Sub | Question | Type |
|---|---|---|
| Q1a | Non-empty string, length multiple of 6; every 6k and 6k+3 position must contain 1 | Positional + length constraint |
| Q1b | Starts with 01 and does not contain 110 as a substring | Prefix condition + substring avoidance |
| Q2a | DFA for L1* where L1 = {110} | Kleene star of a 3-char string |
| Q2b | DFA for L2: first and third positions contain the same symbol (if they exist) | Positional/conditional |
| Q2c | Cross product for L1* ∪ L2, draw at least 5 states | Union cross product, partial accepted |

**Set B — Actual Questions (variant):**

| Sub | Question | Type |
|---|---|---|
| Q1a | Non-empty string, length multiple of 5; every 5k and 5k+3 position must contain 1 | Same type as Set A Q1a, different n |
| Q1b | Starts with 10 and does not contain 010 as a substring | Same type, different prefix/substring |
| Q2a | DFA for L1* where L1 = {101} | Same type, different string |
| Q2b | DFA for L2: first and third positions contain different symbols (if they exist) | Same type, inverted condition |
| Q2c | Cross product for L1* ∪ L2, draw at least 5 states | Same |

**Observed TNF tendencies (Quiz 1):**
- Pattern announced in advance and matched exactly — TNF is transparent about quiz structure.
- **Two sets exist (A and B):** Same question structure, parameters changed (n, strings, symbol condition). Prepare for the type, not the specific values.
- Cross product was built from Q2's own DFAs (L1* and L2). Do not treat DFA construction and cross product as independent — they feed each other.
- Marks split evenly: 10 marks pure DFA (Q1), 10 marks DFA + cross product (Q2).
- No definition/theory-only questions — all marks were for construction.
- **Kleene star appeared (Q2a):** L1* where L1 is a 3-character string. This is harder than simple modular counting — it is a regular operation. Kleene star DFA construction must be prepared for future quizzes.
- **Q1 difficulty is higher than prep examples:** Positional constraints (every 6k and 6k+3) require tracking position within a cycle and rejecting when a required position does not hold. Simple mod-n counting is not sufficient — you also need constraint enforcement per position.
- **Q1b type:** Prefix match + substring avoidance. Two simultaneous conditions in one DFA. States must track both the prefix status and the longest suffix of the forbidden string matched so far.
- **Q2c partial credit:** "At least 5 states" — TNF explicitly allows partial cross product tables. Draw the reachable states in order from the start state; 5 is the minimum.
- **Regular expressions excluded** despite being in original syllabus announcement.
- **Result:** 20/20. TNF was lenient (first quiz of semester). Do not assume this grading standard continues in later quizzes.
- **PYQ:** `CSE331/pyq/CSE331_SM26_Quiz1_Solutions.pdf`

---

### Quiz 2 — Confirmed Pattern (2026-07-22, Section 16)

> Note: this quiz was skipped due to illness (see `task-history/tasks-history.md` #16) — pattern documented here from the official quiz paper and TNF's class feedback, for Mid-Term prep, not from a personal sitting.

**Syllabus:** Regular expressions from language descriptions; RE for complements; RE for set combinations (union/intersection) via De Morgan's-style identities; FA→RE via state elimination
**Total marks:** 20
**Two sets (A and B), identical structure, different languages:**

| Question | Content | Marks |
|---|---|---|
| Q1 | 3 sub-parts: (a) RE for L1, L2, L3 given as plain-language descriptions (b) RE for the complement of each (c) RE for two set-combination expressions | 3 + 3 + 4 = 10 |
| Q2 | Convert a given 3-state FA to a RE via state elimination, in a specified order; must show the state diagram after each elimination; exactly one start and one accept state at the end | 10 |

**Set A:** L1 = starts & ends with 1 (edge case: length 1); L2 = length ≥ 4 (done in class); L3 = contains `10` as substring (done in class). Elimination order: q2, q0, q1.
**Set B:** L1 = starts & ends with 0 (edge case: length 1); L2 = length ≤ 5 (done in class); L3 = contains `01` as substring (done in class). Elimination order: q1, q0, q2.

**Q1c pattern:** Solved via De Morgan's-style identities — complement of intersection = union of complements, complement of union = intersection of complements — reducing to the complements already found in (b) rather than re-deriving from scratch.

**TNF feedback on grading (2026-07-24 announcement):**
- Do not wrap the entire regular expression in a Kleene star unless the question specifically requires it — a recurring mistake this round.
- **Q2 (state elimination) — main source of lost marks:** when eliminating a state, only *that* state's own self-loop is folded into the new transition — self-loops on states *not* being eliminated must not be pulled in. Only two paths feed each new transition: the direct path between two states, and the path routed through the state being eliminated. Considering other paths or unrelated self-loops produced longer, incorrect final REs.
- Marks published for everyone except students who used the **state equation method** for Q2 — TNF will verify the method's source before deciding on the remaining marks for that group.
- **Resolved (TNF, 2026-07-26):** the state equation method (and Arden's Theorem generally) is **not** the state elimination method. TNF gave half marks on this quiz as a one-time leniency even though the resulting answers were wrong — **this will not be accepted on the Mid-Term.** State elimination is the only accepted method for FA→RE conversion going forward.

**PYQ:** `CSE331/pyq/CSE331_SM26_Quiz2_Solutions.pdf`

---

### Quiz 3 & Quiz 4 — Confirmed Syllabus (TNF announcement, received 2026-08-12)

**Date:** 2026-08-24 (Monday), 8:20 AM — both quizzes, same day, confirmed intentional

**Confirmed syllabus:**
- Pumping Lemma
- CFG parse trees, derivations, ambiguity
- Designing CFG

**Pattern hint (boxed margin note, own handwritten lecture 14 scan, 2026-08-12):** "Quiz question pattern — question similar to what was practiced during class." Written right after that lecture's CFG design worked examples — consistent with the confirmed syllabus above, now a secondary pattern signal rather than the primary source. No worked example or PYQ attached.

**Scope clarification (TNF, Discord, 2026-08-13, answering a student's question):** Regex→CFG will not be asked as a direct "transform this regex into a CFG" question. It will instead be posed as: given a regular language, find the regex yourself, then write the CFG from it — i.e. the regex step is scaffolding the student is expected to supply, not a given. Falls under "Designing CFG" above, not a separate topic.

**Actual format (sat 2026-08-24):** Each quiz had 2 questions, each with 2–3 subparts — not 3 questions as the announcement's syllabus list length might suggest. Quiz 3: Q1 Pumping Lemma, Q2 CFG. Quiz 4: both questions CFG. Self-assessed result: weak on Quiz 3 Q1 specifically (Pumping Lemma not learned properly beforehand), otherwise solid on CFG across both quizzes. **Result:** Quiz 3 — 13.5/20; Quiz 4 — 18/20. Confirms the self-assessment — Quiz 3 (Pumping Lemma gap) scored meaningfully lower than Quiz 4 (CFG only). Full archive entries: `task-history/tasks-history.md` #29 (Quiz 3), #33 (Quiz 4).

**PYQ:** `CSE331/pyq/CSE331_SM26_Quiz3_Quiz4.pdf` (Set A + B, both quizzes, one file)

---

### Quiz 5 — Confirmed Pattern (TNF official announcement)

**Date:** 2026-09-07 (Monday), 8:20 AM — **CONFIRMED**, official announcement (supersedes the earlier "time TBC" tracking)
**Syllabus:** PDA (Pushdown Automata) only. **Two-stack PDA explicitly excluded** — TNF's own note, also applies to the Final (see Final Pattern above).
**Confirmed via Discord screenshot (2026-08-31, 10:07 AM message):** "📢 Quiz 5 Announcement — 📅 Date: 7 September, Monday — 🕐 Time: 8:20 AM — 📝 Syllabus: PDA — Note: Two stack PDA is NOT included in the final syllabus or the quiz syllabus."

**Decidability-proof theorem hints — separate message, NOT part of the Quiz 5 announcement (correction, 2026-09-05):** posted by TNF the same day but ~2h48m later (12:55 PM, standalone, no reference to "quiz" or Quiz 5 in the message itself): "For proof of Decidability problems, the following theorems from book can be helpful. Chapter 4. Theorems: 4.1, 4.2, 4.3, 4.4, 4.5, 4.11 (proof in page 207 for theorem 4.11)." Previously mis-recorded as "same announcement" as the Quiz 5 syllabus — corrected after re-checking the source screenshot. Treat this as general/assignment-directed guidance, not confirmed Quiz 5 syllabus: it directly matches CSE331 Assignment 2 Part B's decidable-language closure proofs (Union, Complement) and reduction proofs (A_TM, E_TM undecidability) — same book chapter, same theorem family — so cross-reference these theorems when working that assignment, not for Quiz 5 prep.

**Bonus questions (TNF, separate announcement):** optional, not mandatory. Theoretical/basic concepts of CFG only (as covered in class) — no construction expected for the bonus portion. TNF's suggested prep: the textbook or class notes on CFG fundamentals.

**Actual format (sat 2026-09-07):** Two sets (A and B), identical structure. Q1: 3 PDA design sub-parts, 20 marks total. Q2 (bonus): 3 CFG true/false-with-justification sub-parts, ungraded toward the 20 but scored separately.

| Sub | Set A | Set B |
|---|---|---|
| Q1a | w1#w2 — w1 is all-0s with an odd count, w2 contains `011` as a substring | w1#w2 — w1 is all-0s with an even count, w2 contains `101` as a substring |
| Q1b | 0^i 1^j 2^k, i,j,k ≥ 0, j > 2i + k | 0^i 1^j 2^k, i,j,k ≥ 0, j > i + 3k |
| Q1c | w1w2, \|w2\|=2\|w1\|, odd number of 1s **in w1** | w1w2, \|w2\|=2\|w1\|, even number of 1s **in w2** |
| Bonus 2a | T/F: CFLs are closed under union, complement, and intersection | (same, both sets) |
| Bonus 2b | T/F: given `A → aB`, `B → aBb \| a`, is `aab` derivable from `B → aBb → aab`? | (same, both sets) |
| Bonus 2c | T/F: regular languages are a subset of CFLs | (same, both sets) |

**Erratum + grading note on Q1c (TNF, verbal, 2026-09-07):** Set A's Q1c originally read "odd number of 1s in w1" — should have read "...in w2," matching the property's position in Set B (the second, popped-against substring, not the first). TNF confirmed Set A's Q1c (property on w1, the pushed/read-first substring) is harder to construct than Set B's equivalent (property on w2), so **she will apply leniency on Q1c specifically for Set A sitters** who lost marks there — not a blanket regrade of the quiz.

**User's set:** A. **Result (official, confirmed 2026-09-09): 19.5/20.** Self-assessed going in was Q1(a)–(c) all correct (including the harder Set A Q1c) with only Bonus 2a (CFL closure) missed — since the bonus doesn't count toward the 20, a 0.5-mark deduction exists somewhere inside Q1 that the self-assessment didn't catch; not identified which sub-part, and not worth chasing given the size. Bonus 2b and 2c self-assessed correct.

**PYQ:** `CSE331/pyq/CSE331_SM26_Quiz5.pdf`

---

## Assignment Pattern

**Weight:** 10% (2 assignments, Part 1 + Part 2 each)
**Created by:** TNF
**Assignment 1 Part A:** DFA design, NFA→DFA conversion. Deadline: 2026-07-08 11:59 PM.
**Assignment 1 Part B:** Regex, RE→NFA, DFA→RE via state elimination. Deadline: 2026-07-24 11:59 PM. TNF released feedback 2026-07-26 for students who had submitted by then.

**PYQ reference:** `CSE331/pyq/other-faculty-assignments/` — past assignments from multiple faculty sections, see [pyq_index](pyq/pyq_index.md)

> [!confirmed] **Component finalized: 4.75/10 — confirmed 2026-09-17 via the official CSE331 marks Google Sheet.** Both Assignment 1 parts (A and B) were missed (`task-history/tasks-history.md` #12) → Assignment 1 total = 0/10. Assignment 2 Part A = 4.5/5 (post-penalty, #36), Part B = **5/5** (newly confirmed, #48) → Assignment 2 total = 9.5/10. Overall component = average of the two assignments' totals = (0 + 9.5)/2 = **4.75/10**, no components still pending.

### Assignment 1 Part B — Common Mistake Patterns (aggregated, anonymized; TNF class feedback, 2026-07-26)

> TNF distributed a per-student feedback sheet naming individual mistakes for everyone who had submitted. That sheet names ~29 classmates by ID — it is not reproduced here; only the recurring *pattern* across the class is kept, since that's the transferable study signal. The raw sheet is not shared.

**Q1 (regular expressions for described languages) — most common errors:**
- Wrapping the **entire** expression in a Kleene star when only a sub-part needed it — this was the single most frequent mistake, and it silently breaks constraints like "guarantee an odd number of 0s after the last 1."
- Missing edge cases at very short lengths (length 0 or 1).
- Overlooking that a required symbol/substring could be preceded by an extra occurrence of the same symbol, or that consecutive repeats could sneak in where the language forbids them.
- Length constraints framed as "multiple of n" — frequently mishandled (not enforcing the multiple, or wrongly requiring length *greater than* n as well).
- Missing a second `*`-quantified term when the expression has more than one independent repeating part.

**Q2 (NFA-related construction) — recurring error:** incorrect conversion between representations; a specific sub-language component not being detected by the constructed answer.

**Q3 (FA→RE via state elimination) — the weakest area class-wide, consistent with the Quiz 2 pattern above:**
- Transitions missed, or transitions wrongly added from states that were **not** the one being eliminated in that step (same mistake flagged on Quiz 2).
- Forgetting to remove the auxiliary new start/accept state at the end — the final RE must come from exactly one start state to exactly one accept state.
- Missing ε-transitions or a missing Kleene star in the final answer.
- Overcomplicating the result with extra states, transitions, or Kleene stars that aren't actually part of the solution.
- Wrong order of concatenation in the final step.
- A minority got this fully correct; a few did not attempt it at all.

**Overall (TNF, 2026-07-26):** many students are still struggling with FA→RE conversion specifically — it needs dedicated practice ahead of the Mid-Term, where RE↔FA equivalence and DFA→RE conversion are both confirmed syllabus (see Midterm Pattern above), and where the state elimination method rule above is enforced without the quiz's leniency.


<!-- Add pattern notes here. Note that past assignments vary by faculty — filter for TNF-authored ones if available. -->
