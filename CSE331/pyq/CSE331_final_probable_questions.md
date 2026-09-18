---
type: pyq-analysis
course: CSE331
exam: final
status: complete
date: 2026-08-12
---

# CSE331 — Final Probable Questions & PYQ Pattern Analysis

**Exam:** 2026-09-15, 4:30–6:30 PM | **Weight:** 35% | **Created by:** Central (BRACU question committee) — not TNF-authored, see [[cse331_exam_notes]].

**Syllabus status:** TNF has not yet announced a confirmed Final syllabus (as of 2026-08-12). `cse331_exam_notes.md` currently only points to the full topic map ([[cse331_topics]], Topics 1–22) as a placeholder. This analysis is **PYQ-evidence-based, not syllabus-confirmed** — the topics below are what has actually appeared across all sittings collected, not an assumption that every post-Midterm topic in the map is in scope. Re-check against a formal TNF announcement once made.

**Sources reviewed:** 3 sittings on HDD (`Course-Library/cse/cse321/pyq/` → `cse331/pyq/`), 6 files total (Set A + Set B each). **All 6 read in full detail** — Spring 2025, Fall 2025, Spring 2026. This is every Final PYQ currently collected; no sampling was needed. **Rigorously re-cross-checked against the source PDFs 2026-09-11** — two numerical errors from the original 2026-08-12 pass were found and corrected (a CFG sub-part-count claim wrongly generalized to Spring26, and a CO3 percentage-of-paper claim that didn't hold for Spring25); everything else checked out.

---

## Direct Answer: Did the Questions Change, or Follow a Pattern?

**Strong, stable skeleton with one structural break at Spring 2025 → Fall 2025.** Unlike CSE321's Final (which changed shape substantially), CSE331's Final looks like the *same six modules* tested every sitting, just reordered and re-weighted slightly between Fall 2025 and Spring 2026.

**Spring 2025 (older format):**
- **6 problems total, solve 5** (Problem 6 bonus). Duration 110 minutes.
- No True/False section at all.
- CFG design and PDA design are both worth **15 points each** (the two largest sections by far) — CFG design in particular has 6 sub-parts including a language *intersection* (L4 = L1 ∩ L3) and a length-matched-concatenation language (L5), noticeably harder than later sittings' CFG problems.
- Turing Machines is a small 5-point problem (one short conceptual sub-question + one TM design).

**Fall 2025 and Spring 2026 (current format, identical marks skeleton, different problem order):**
- **7 problems total, solve 6** (Problem 7 bonus). Duration extended to 1 hour 50 minutes (same 110 min, just labeled differently).
- A dedicated **5-point True/False section** appears (absent in Spring25) — always 5 statements, "write True/False, and the correct statement if False."
- CFG design drops to **10 points**. Sub-part count is *not* uniform across the current format: Fall25 dropped to 5 sub-parts (a–e), but Spring26 kept all 6 (a–f), just at lower point values (2+2+1+2+1+2=10 vs. Spring25's 3+3+3+1+2+3=15). What both current-format sittings share is that the intersection/concatenation-heavy variants from Spring25 are gone.
- PDA design drops to **10 points** (3 sub-parts, same structure as Spring25's but worth less).
- Turing Machines grows to **10 points**, including — Spring26 only — an explicit **TM configuration read-off** sub-question ("given `000q2 11`, identify tape contents, state, head position") that didn't appear in Fall25 or Spring25.
- **Problem order differs between Fall25 and Spring26** even though the point-value skeleton is identical: Fall25 runs Pumping Lemma → CFG → T/F(CFL) → Parse Trees → PDA → TM(+T/F+proof); Spring26 runs Pumping Lemma → CFG → TM(+config) → Parse Trees → T/F(Non-Regular) → PDA. Don't anchor revision order to problem *position* — anchor to the module list itself.

**Constant across all three sittings, no exception:**
- **Problem 1 is always Pumping Lemma (non-regularity proof), always CO5, always exactly 5 points.** The single most reliable fixture on this exam.
- **Derivations / Parse Trees / Ambiguity is always exactly 10 points**, in every sitting, regardless of the older/newer format split. Always the same 4-part structure: (a) leftmost derivation for a given string, (b) parse tree for that derivation, (c) two *more* parse trees proving ambiguity, (d) a short string with exactly one parse tree. This is the most stable single problem on the whole exam.
- **CFG design and PDA design are always present**, both drawing from 3–4 defined languages with sub-parts (a)/(b)/(c)/... — only the point value and sub-part count shrank between Spring25 and the current format.
- **The bonus problem is always Pumping-Lemma- or decidability-flavored** and worth 3–5 points — cheap, skippable without real cost, but note Spring25's bonus was a full TM design (harder) versus Fall25/Spring26's shorter proofs.

**Verdict:** treat Fall25 and Spring26 as the current, load-bearing format (7 problems, 6 required, T/F section present, CFG/PDA at 10 pts each). Spring25 is one format-generation older — still useful for CFG design practice (its problems are *harder*, good stretch practice) and for confirming that Parse-Tree/Ambiguity and Pumping-Lemma-as-P1 predate the format change, but don't expect its 15-point CFG/PDA weighting or missing-T/F-section structure to recur.

---

## Marks Distribution by Module (Approximate)

| Module | Spring 2025 | Fall 2025 | Spring 2026 | CO tag |
|---|---|---|---|---|
| Pumping Lemma (non-regularity) | 5 | 5 | 5 | CO5 |
| CFG Design | 15 | 10 | 10 | CO3 |
| Derivations / Parse Trees / Ambiguity | 10 | 10 | 10 | CO3 |
| PDA Design | 15 | 10 | 10 | CO3 |
| Turing Machines (design + short concept) | 5 | — | 10 | CO4 |
| True/False section (CFL properties or Non-Regular Languages) | — | 5 | 5 | CO3 |
| TM True/False + decidability proof | — | 10 | — | CO4 |
| Bonus | 3 | 3 | 5 | CO4/5 |

The Fall25/Spring26 total redistributes the 5 points Spring25 spent on a bigger CFG+PDA into a dedicated T/F section plus a larger Turing Machines problem — net effect: **in the current format (Fall25, Spring26), CO3 (context-free material: CFG, ambiguity, PDA, CFL T/F) is exactly 70% of the paper both times** (35 of 50); **Spring25's older format is higher, at 80%** (40 of 50 — CFG15+Parse10+PDA15 — since it had no T/F section drawing points away from CO3). Across all 3 sittings, **CO4 (Turing Machines/decidability) is 10–20%**; **CO5 (Pumping Lemma) is a fixed 10%** (5 of 50, every sitting).

---

## Question-Type Pattern per Module

**Pumping Lemma (always Problem 1, CO5, 5 pts)** — one language given, prove non-regular via pumping lemma. Languages are alphabet-mixed (often `{0,1,2}*` with arithmetic relations between symbol counts, e.g. Spring26: "if i=1 then j=k, otherwise j≠k"). Set A/B swap the arithmetic condition or the length relation but keep the same proof *shape*. The bonus problem is frequently a second pumping-lemma proof on a different language (e.g. "n is a composite number" — appeared identically in both Fall25 sets).

**CFG Design (always Problem 2 in current format, CO3, 10–15 pts)** — 3–5 related languages defined up front (often building on each other: L3 defined in terms of L1/L2, or L4 = some combination of earlier ones), then a grammar requested for each. Recurring sub-question types:
- A "write all strings of length N" enumeration sub-part worth 1 point — cheap, don't skip it.
- Positional constraints ("contains 0 in every second and third position") — same style as the Quiz 1 DFA positional questions per [[cse331_exam_notes]], just now expressed as a CFG.
- A language built from two languages via a **counting relationship** ("number of 0s in w1 is twice the number of 1s in w2") — appeared in Spring26 both sets, a genuinely CFG-specific skill (DFAs can't count/compare across an unbounded range) worth deliberate practice, not just carried over from DFA-era intuition.
- Spring25 additionally tested **intersection of two CFLs** (L1 ∩ L3) — not retested in Fall25/Spring26, but intersection-of-CFG-languages is a real Sipser gap (CFLs aren't closed under intersection in general, so this only works because of the specific structure) worth understanding conceptually even though it hasn't recurred.

**Derivations / Parse Trees / Ambiguity (always present, CO3, exactly 10 pts, identical 4-part structure every sitting)** — one grammar given (2–3 nonterminals, `S → ...` with a repeating-block production plus a terminal/near-terminal production), then: leftmost derivation for a given string (3 pts) → its parse tree (2 pts) → **two more distinct parse trees for the same string**, proving ambiguity (4 pts) → a short string with exactly one parse tree (1 pt). The grammars across all 3 sittings share a family resemblance: a "prefix/infix generator" nonterminal (`D`/`P`) that recursively wraps pairs of terminals, composed with a second nonterminal (`Q`) that generates a separately-structured suffix. Set A/B differ only in the terminal alphabet used in productions and the target string. **This is the single highest-confidence, highest-consistency problem on the entire exam** — practice the "find 2 additional parse trees" skill specifically, since it's the hardest sub-part (4 of the 10 points) and depends on spotting *where* the grammar allows a derivation-order swap, not just executing a known algorithm.

**PDA Design (always present, CO3, 10–15 pts, 3 sub-parts of ~3–5 pts each)** — same format every sitting: 3 related languages, PDA state diagram requested for each. Recurring types: a `#`-separated two-half language with a length/count relationship between the halves (near-universal — every sitting has at least one), a reversal language (`w#w^R` or `w1#w2#w1^R`), and an arithmetic-relation-over-symbol-counts language similar in style to the Pumping Lemma languages. Set A/B swap which count is odd/even, which direction an inequality points, or which symbol is being counted — same underlying construction.

**Turing Machines (present every sitting, but format shifted — CO4, 5–10 pts)** — TM design for a language (often reusing an arithmetic-relation language style from earlier problems, now decided rather than recognized), plus one short conceptual sub-question. Spring26 introduced a **TM configuration read-off** sub-question (given a string like `000q2 11`, state the tape contents/state/head position) not seen in the other two sittings — low-cost to prepare (pure definition-reading, no construction) and worth knowing given it's the newest addition. A recurring theoretical sub-question across sittings: "is a Turing-recognizable language's *complement* also recognizable" (Spring25) and "prove L1·L2 is decidable given L1, L2 decidable" (Fall25 both sets) — these are Topic 20 (decidable-language closure) results, not full TM constructions, and are cheap points if the relevant closure proof is memorized.

**True/False sections (Fall25 + Spring26 only, CO3/CO4, 5 pts, 5 statements each)** — "write True/False, and the correct statement if False." Content varies by sitting (Fall25 = CFL properties: PDA determinism, acceptance-with-nonempty-stack, CFL-non-regularity, complement-closure, valid-CFG-form; Spring26 = Non-Regular-Languages: the RL⊂CFL⊂recognizable⊂decidable chain, pumping-lemma-satisfying-non-regular-languages, recognizable-vs-decidable halting behavior, PDA-recognizes-all-languages, ambiguity-of-regular-language-grammars) — but the **format and difficulty level are stable**: each is a common misconception from the course's core theorems, not an obscure edge case. Both sittings' actual statements are listed above in the Direct Answer section's format description; use them directly as a self-test bank since T/F questions like these tend to recur in spirit even when reworded.

---

## Set A vs. Set B Pattern

Same finding as CSE321's Final and CSE331's own Midterm/Quiz analyses: **every sitting is structurally parallel between Set A and Set B** — identical problem slots, identical mark allocations, identical grammar *shapes* — with only the specific languages, strings, symbols, or inequality directions swapped (e.g. Spring26 P1: `j=k`/`j≠k` condition unchanged, only which digit triggers the exception; Fall25 True/False: same 5 statement *topics*, reordered and reworded but testing the same misconceptions; PDA problems: which count is odd vs. even flipped). Practicing one set is direct practice for the other set's identical skill — same conclusion as the Midterm and Quiz pattern notes in [[cse331_exam_notes]].

---

## Probable Questions for the Final — Ranked

<!-- Ranking rationale: frequency and stability across all 3 sittings sampled (this is the full PYQ population currently collected, not a sample). -->

**Near-certain — appeared in every sitting sampled, same point value every time:**

1. **Pumping Lemma non-regularity proof** — Problem 1 every sitting, always 5 points, always CO5. Language usually mixes 2–3 symbols with an arithmetic/counting condition.
2. **CFG derivation + parse tree + ambiguity (2 extra parse trees) + one-parse-tree string** — the exact 4-part, 10-point structure appeared unchanged in all 3 sittings. The single most reliable non-Problem-1 fixture on the exam.
3. **CFG design from 3–5 related languages**, including at least one positional-constraint or counting-relationship language and a 1-point "enumerate strings of length N" sub-part.
4. **PDA design from 3 related languages**, including at least one `#`-separated two-half language with a count/length relationship and one reversal-based language.

**High probability — present in 2 of 3 sittings, or present in both current-format sittings:**

5. **A 5-point True/False section** on either CFL closure/PDA properties or the regular⊂CFL⊂recognizable⊂decidable hierarchy and pumping-lemma edge cases — present in both current-format sittings (Fall25, Spring26), absent only in the older Spring25 format.
6. **Turing Machine design** for a counting/arithmetic-relation language, paired with a short conceptual sub-question (complement-of-recognizable, or a decidable-language closure proof like L1·L2).
7. **A decidable-language closure proof** ("prove L1·L2 is decidable given L1, L2 decidable" or similar) as either a main-problem sub-part or the bonus.

**Moderate probability:**

8. **TM configuration read-off** (tape contents/state/head position from a string like `000q2 11`) — only in Spring26 so far, but cheap to prepare (pure definition, no construction) given it's the newest recurring-format addition.
9. **A bonus Pumping Lemma proof** on a second, often numerically-flavored language (e.g. "length is a composite number") — appeared identically across both Fall25 sets; low cost, skip only if genuinely out of time.

**Lower probability but still syllabus-legal (per [[cse331_topics]], not yet PYQ-confirmed):**

10. **DFA minimization, Chomsky Normal Form, CFG≡PDA equivalence proof, Pumping Lemma for CFLs / non-CFL proofs, TM variants (multi-tape/nondeterministic), Church-Turing Thesis, full undecidability (A_TM/Halting Problem) or reducibility proofs** — none of these appeared in any of the 3 sittings reviewed, despite being nominally in-scope per the full topic map. Zero PYQ evidence isn't proof they can't appear (small sample — only 3 sittings collected), but they are conspicuously absent from a fairly consistent format across all 3, so they read as low-yield relative to items 1–9.

---

## Revision Priority Implied by This Analysis

Items 1–4 map onto material CSE331's lecture notes have already covered in depth this session (CFG design: lecture 12–14; ambiguity/parse trees: lecture 12) — those are the highest-leverage, most exam-tested material and are also the most recently taught. Items 6–8 (Turing Machines, decidability proofs) are **not yet covered in any transcribed lecture note** as of 2026-08-12 (course is still mid-CFG-unit per `lecture-notes/CSE331_lecture_14_2026-08-12.md`) — flag this as forward material to watch for once TNF reaches Chapter 3–4 of Sipser, not something to self-study cold from PYQs alone. PDA design (item 4) sits in the same position — Sipser Ch 2.2, motivated but not yet reached (lecture 12 ends on the balanced-parentheses grammar that motivates PDAs, per [[cse331_exam_notes]]'s Midterm section note). Re-run this analysis's "syllabus status" check once TNF announces a confirmed Final syllabus, since the current 65–75% CO3 weighting assumes the PYQ pattern holds and no topic gets dropped or added the way the Midterm's scope was later narrowed (NFA→DFA direction-only, state-elimination-only for DFA→RE).
