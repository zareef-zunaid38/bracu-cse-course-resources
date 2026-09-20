---
course: CSE331
type: resource-guide
status: complete
---

# CSE331 — Resources

## Primary Textbook
**Introduction to the Theory of Computation** — Michael Sipser (3rd ed.)
The course follows this book. Definitions, theorems, and proofs in this course are Sipser's unless your faculty states otherwise. Read the relevant chapter section before attending a lecture on that topic.

> [!info] No slides for this course. TNF confirmed verbally — no lecture slides will be provided for Summer 2026. Rely on Sipser, MIT OCW, and in-class notes.
> Partial gap-fill: [Lecture Notes](../lecture-notes/CSE331_lecture_01_prelims.md) (2026-06-08 to 2026-06-29) transcribed from Farhan Labib's handwritten notes, status `needs-review`. Covers Topics 1–3 (Preliminaries through NFA→DFA subset construction).

## Additional Lecture Notes (not included)
TNF shared two extra lecture notes written by other students: one on Turing Machines (Topics 17–19: TM definition and examples, decidability proofs, the Universal TM and the Halting Problem) and one on Reducibility (Topic 22: the gcd/lcm intuition for A ≤ B, and why the direction of a reduction matters for undecidability proofs). They belong to their authors, so they are not hosted here — ask TNF or your classmates for them.

## Class Recordings (MHB Sir) — Online Only
Recordings by Mursalin Habib Sir are online only.
Coverage map (for finding relevant videos):
- DFA Part A & B → Topics 2–3
- NFA Part A → Topics 3–4
- Regular Expression Part A → Topics 5–6
- DFA Minimization → Topic 7
- Pumping Lemma → Topics 8–9
- March/April sessions → Topics 10–16

## Topic-by-Topic Resources

### Foundations (Topic 1)
- Sipser Ch 0 — read in full before Topic 2
- CSE230 Discrete Mathematics notes are direct prerequisites

### Regular Languages (Topics 2–9)
- Sipser Ch 1 — primary
- **Video:** MIT OCW 18.404 — Theory of Computation (Michael Sipser's own lectures, free on MIT OCW)
  Lecture 1–4 cover DFA, NFA, RegEx, and non-regular languages
- **Video:** Neso Academy — Theory of Automata & Computation playlist (YouTube, free)
  Good for worked examples; use alongside Sipser not instead of it
- **Pumping Lemma extras (TNF-shared, 2026-08-03):** cited as secondary/supplementary material in [Lecture 11](../lecture-notes/CSE331_lecture_11_2026-07-21.md). All three are in `CSE331/practice/`:
  - `CSE331_pumping_lemma_20_examples.md` — practice sheet of 20 non-regularity proofs (cleaned up from a YouTube video's raw auto-caption transcript into structured proofs, with a techniques index at the end).
  - `CSE331_pumping_lemma_rephrased_proofs.pdf` — TNF's rephrased proofs (4 problems).
  - `CSE331_pumping_lemma_exercises_solutions.pdf` — 29-problem solved exercise set.

### Context-Free Languages (Topics 10–16)
- Sipser Ch 2 — primary
- MIT OCW 18.404 Lectures 5–8
- Neso Academy — CFG and PDA sections
- For CNF conversion: practice the 4-step procedure (START, TERM, BIN, DEL/UNIT) repeatedly until mechanical

### Computability (Topics 17–19)
- Sipser Ch 3 — primary
- MIT OCW 18.404 Lectures 9–11
- Turing Machines lecture note shared by TNF (not included in this repo). Covers TM 7-tuple definition, state-diagram notation, worked TM constructions (contains-11, 0ⁿ1ⁿ, w#w), configurations, Church-Turing thesis, Turing recognizable vs. decidable, A_DFA decidability, Universal TM (A_TM), and the Halting Problem undecidability proof (diagonalization/self-reference technique).
- Key insight: a TM is just a DFA with an infinite read/write tape. The power comes from the ability to write.

### Decidability (Topics 20–21)
- Sipser Ch 4 — primary
- MIT OCW 18.404 Lectures 12–14
- The Halting Problem proof by diagonalization is one of the most important proofs in CS. Work through it until you can reproduce it from scratch.

### Reducibility (Topic 22)
- Sipser Ch 5.1–5.3 — primary
- MIT OCW 18.404 Lectures 15–17
- Rice's Theorem is the single most powerful undecidability tool — understand its statement and conditions carefully

## Exam Notes
- Theory of Computation exams at BRACU are proof-heavy. You will be asked to:
  1. Design DFAs, NFAs, PDAs, or TMs for given languages
  2. Prove a language is non-regular or non-context-free using the pumping lemma
  3. Prove a language is decidable or undecidable via reduction
- State diagram drawing is required for automata problems. Practice clean, labeled diagrams.
- Formal 7-tuple definitions may be required — memorize the TM tuple structure.
