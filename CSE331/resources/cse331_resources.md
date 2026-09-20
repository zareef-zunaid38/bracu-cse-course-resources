---
course: CSE331
type: resource-guide
status: complete
---

# CSE331 — Resources

## Primary Textbook
**Introduction to the Theory of Computation** — Michael Sipser (3rd ed.)
The course follows this book. Definitions, theorems, and proofs in this course are Sipser's unless your faculty states otherwise. Read the relevant chapter section before attending a lecture on that topic.

> **Info:** No slides for this course. TNF confirmed verbally — no lecture slides will be provided for Summer 2026. Rely on Sipser, MIT OCW, and in-class notes.
> Partial gap-fill: [Lecture Notes](../lecture-notes/CSE331_lecture_01_prelims.md) (2026-06-08 to 2026-06-29) transcribed from Farhan Labib's handwritten notes, status `needs-review`. Covers Topics 1–3 (Preliminaries through NFA→DFA subset construction).

## Lecture Notes Shared by TNF
`CSE331/shared-notes/` — Turing Machines (covers topics 17–19), by Rafiad Sadat Shahir (RSS), TNF-shared. Two versions kept, content differs:
- `CSE331_lecturenote_rss_L10_turing_machines.pdf` (2025 original) — TM definition/examples, "Difference with Finite Automata" list, A_DFA decidability proof, **A_REX decidability proof** (regex→NFA→DFA→simulate), Universal TM, Halting Problem undecidability.
- `CSE331_lecturenote_rss_L10_turing_machines_v2_2026.pdf` (received 2026-08-25, LaTeX-retypeset) — same core content minus the "Difference with FA" list, but swaps A_REX for **E_DFA decidability proof** (reachability-marking algorithm). Otherwise matches v1.
- RSS's affiliation to the course is unconfirmed; content trusted as accurate on TNF's authority as the sharing source.

`CSE331_lecturenote_rifat_reducibility.pdf` (received 2026-09-02, TNF-shared, by Md. Rifat Rahman, June 2026) — Reducibility (Topic 22): intuitive gcd/lcm framing of A ≤ B, general reduce-to-known-solved-problem strategy, why the direction matters for undecidability proofs (reduce a *known* undecidable problem *into* the unknown one, never the reverse), A_NFA/A_REX decidability via reduction to A_DFA, and the two worked undecidability reductions used directly in [Lecture 19](../lecture-notes/CSE331_lecture_19_2026-09-02.md): A_TM undecidable (reduced from HALT_TM) and E_TM undecidable (reduced from A_TM). Used to resolve Lecture 19's diagram ambiguity.

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
- RSS lecture note (both versions — see *Lecture Notes Shared by TNF* above). Covers TM 7-tuple definition, state-diagram notation, worked TM constructions (contains-11, 0ⁿ1ⁿ, w#w), configurations, Church-Turing thesis, Turing recognizable vs. decidable, A_DFA decidability, Universal TM (A_TM), and the Halting Problem undecidability proof (diagonalization/self-reference technique).
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
