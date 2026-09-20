---
course: CSE331
type: model-question-set
status: in-progress
assessment: Final
scope-confirmed: 2026-09-11
---

# CSE331 — Final Exam: Model Question Set

**Why this exists:** unlike CSE370, CSE331's Final *does* have PYQs — 3 sittings collected (Spring25, Fall25, Spring26, Set A+B each), fully analyzed in [CSE331_final_probable_questions](../pyq/CSE331_final_probable_questions.md). Real PYQ problems are the primary basis per the Grounding Rule below. The one exception is **Reducibility** (syllabus item 8): confirmed by TNF as new this semester with **zero past-paper precedent** — that slot is built as a fresh, fully-solved derivation instead (see Slot 8), following the reduction proofs already worked in [CSE331_lecture_19_2026-09-02](../lecture-notes/CSE331_lecture_19_2026-09-02.md).

**Style anchor:** **Spring 2026 Set A** (read in full from the PDF, 2026-09-14) — the current-format paper closest to this sitting, same creator pool (Central/BRACU, confirmed in the Assessment Creator Map — valid anchor since Final↔Final, not a cross-creator borrow). Cross-checked against Fall 2025 for format stability (identical marks skeleton) via [CSE331_final_probable_questions](../pyq/CSE331_final_probable_questions.md); Spring 2025 (older format, 15-pt CFG/PDA, no T/F section) used only as secondary stretch material, not the primary anchor.

## Scope

**Confirmed in-scope** (`cse331_exam_notes.md`, TNF official announcement received 2026-09-11):
1. Pumping Lemma for Regular Languages
2. Context-Free Languages (CFL)
3. Context-Free Grammars (CFG)
4. Parse Tree, Derivation, and Ambiguity
5. Pushdown Automata (PDA)
6. Turing Machine (TM)
7. Decidability and The Halting Problem
8. Reducibility

**Confirmed exclusions** (per `cse331_exam_notes.md` Final Pattern section): DFA/NFA/RE/subset-construction/closure/minimization (everything before Pumping Lemma), two-stack PDA, Chomsky Normal Form, Pumping Lemma for CFLs / non-context-free proofs, TM Variants (multi-tape, nondeterministic-TM equivalence), Church-Turing Thesis.

**Style anchor used:** Spring 2026 Final, Set A — verbatim structure below. **Note on point weighting:** the real Spring26/Fall25 papers total 50 across 6 required problems + a 5-pt bonus, with no dedicated Reducibility problem (it wasn't examinable then). Since Reducibility is newly confirmed in-scope for this sitting, the actual 2026-09-15 paper's point allocation for it is **unconfirmed** — Slot 8 below is built and practiced regardless of where it lands on the real paper, per TNF's own "treat as high-priority" framing (`cse331_exam_notes.md`).

## Structure

| Slot | Section | Variants (all fully drafted) |
|---|---|---|
| 1 | Pumping Lemma for Regular Languages (item 1) | (a), (b) — both anchored to Spring26 Problem 1's arithmetic-condition style |
| 2 | Context-Free Languages & CFG Design (items 2–3) | (a)–(f), anchored to Spring26 Problem 2's 4-language chain |
| 3 | Parse Tree, Derivation, Ambiguity (item 4) | (a)–(d), the fixed 4-part structure, anchored to Spring26 Problem 4 (same grammar, new target string) |
| 4 | Pushdown Automata (item 5) | (a)–(c), anchored to Spring26 Problem 6 |
| 5 | Turing Machine (item 6) | (a)–(d), anchored to Spring26 Problem 3 |
| 6 | True/False — CFL / Non-Regular Language properties (cross-cutting, recurring exam fixture) | (a)–(j), anchored to Spring26 Problem 5 + Fall25 Problem 3 |
| 7 | Decidability and The Halting Problem (item 7) | (a)–(b), closure proof + DFA-property proof, anchored to Sections B1/C2 of [CSE331_decidability_reducibility_pyq_practice](../practice/CSE331_decidability_reducibility_pyq_practice.md) |
| 8 | Reducibility (item 8) | (a) — **no PYQ anchor**, fresh derivation following the $A_{TM}$/$E_{TM}$ reduction discipline in [CSE331_lecture_19_2026-09-02](../lecture-notes/CSE331_lecture_19_2026-09-02.md) |

## Grounding rule

Every variant must trace to a verified source before it is posed to the user — never invented and shown live:
- **A real PYQ problem**, if available — reuse its structure and style directly; change surface numbers only if the new numbers are independently re-solved and checked.
- **A solved practice-sheet problem** — reuse its structure the same way.
- **A verified worked example already sitting in the course's own concept note** — reuse it via an **order-preserving relabeling** (same relative comparisons/logic, different surface numbers/names) when a fresh derivation isn't necessary for pedagogical variety, or do a fresh derivation — fully solved and checked before posing — if no reusable structure exists for that slot (this is Slot 8's case).

This exists because a wrong worked example actively misleads (Teaching Mode's verify-worked-examples rule) — a model question set is exactly the kind of artifact that gets trusted at face value during revision, so the bar is higher, not lower, than an in-chat explanation.

## Delivery

**Drafting (complete):** every slot and variant is drafted in full below, each independently solved and verified before being written down — no placeholder stubs remain. Every variant also carries a **Difficulty** tag (Easy/Medium/Hard), judged relative to this course's own PYQ pattern history.

**Solving pace:** present and solve one slot's cluster at a time in conversation, not the whole set at once. After the user attempts a variant, check it and record the verified answer directly in this file before moving to the next slot — the questions are complete from the start, but the answers remain a living record filled in as you go.

## Questions

### Slot 1 — Pumping Lemma for Regular Languages

Real anchor (Spring26 Set A, Problem 1, CO5, 5 pts): "L = {0^i 1^j 2^k : i,j,k ≥ 0 and if i=1 then j=k, otherwise j≠k}. Prove that L is not regular using the pumping lemma." Both variants below keep the exact proof *shape* (pick the boundary case where the special condition holds, pump the fixed-size block, break the condition) but change which count triggers the condition and how the relation is stated — both independently solved and checked before being posed.

**(a)** Let L = {0^i 1^j 2^k : i, j, k ≥ 0 and if i = 2 then j = k, otherwise j ≠ k}. Prove that L is not regular using the pumping lemma.

**Difficulty:** Medium — standard proof shape (pick the boundary case where the special condition holds, pump the fixed block, break the condition), but Problem 1's own condition-swap requires re-deriving which witness string actually isolates the special case.
**Status:** posed 2026-09-14, awaiting attempt.
**Answer:** _(pending)_

**(b)** Let L = {0^i 1^j 2^k : i, j, k ≥ 0 and if j is even then i = k, otherwise i ≠ k}. Prove that L is not regular using the pumping lemma.

**Difficulty:** Medium — same proof shape as (a), but the special condition is keyed off the *middle* block's parity rather than an exact value of the *first* block, which changes which witness string isolates it.
**Status:** posed 2026-09-14, awaiting attempt.
**Answer:** _(pending)_

---

### Slot 2 — Context-Free Languages & CFG Design

Real anchor (Spring26 Set A, Problem 2, CO3, 10 pts — read in full from the PDF, `CSE331_Final_Spring26_SetA.pdf`): four related languages (L1 an arithmetic-inequality language over 3 blocks, L2 a periodic positional constraint, L3 a `#`-separated counting-relationship pair, L4 = strings of the form `w#w^R ∈ L3` with the extra constraint `w ∈ L2`), then (a)–(f): grammar/enumerate sub-parts at 2+2+1+2+1+2 = 10 points. The variant below keeps the exact same 4-language chain and sub-part shape, with new languages — each grammar independently derived and verified (not reused from any answer key, since none was available) before being posed.

Consider the following languages:

$$L_1 = \{0^i1^j2^k : i,j,k \geq 0 \text{ and } k > i+j\}$$
$$L_2 = \{w \in \{0,1\}^* : w \text{ contains } 1 \text{ in every second and third position}\}$$
$$L_3 = \{w_1\#w_2 : w_1, w_2 \in \{0,1\}^* \text{ and the number of 1s in } w_1 \text{ is twice the number of 0s in } w_2\}$$
$$L_4 = \{w\#w^R \in L_3 : w \in L_2\}$$

**(a)** Give a context-free grammar that generates $L_1$. [2 marks]

**(b)** Give a context-free grammar that generates $L_2$. [2 marks]

**(c)** Write four strings of length ten in $L_3$ that end with `1#1010`. [1 mark]

**(d)** Give a context-free grammar that generates $L_3$. [2 marks]

**(e)** Write all strings of length less than eight in $L_4$. [1 mark]

**(f)** Give a context-free grammar that generates $L_4$. [2 marks]

**Difficulty:** Hard — (a)/(b) are Medium (direct block/positional constructions), but (c)–(f) require the genuinely CFG-specific "counting relationship across a separator" skill the PYQ analysis flags as needing deliberate practice, not carried over from DFA-era intuition.
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

---

### Slot 3 — Parse Tree, Derivation, and Ambiguity

Real anchor (Spring26 Set A, Problem 4, CO3, 10 pts): the exact grammar $S \to PbQ$, $P \to aP \mid bP \mid \varepsilon$, $Q \to aQa \mid aQb \mid bQa \mid bQb \mid \varepsilon$, target string `abaabbab` — the fixed 4-part structure (leftmost derivation, its tree, 2 more trees proving ambiguity, a length-4 string with exactly one tree) appears unchanged in all 3 sittings sampled, always 3+2+4+1 = 10 points. This variant reuses the **identical, already-taught grammar** (it's the defining fixture of this problem slot, not something to re-derive) with a **new target string**, independently parsed and verified below.

**Mechanics used to build and check this variant** (so the reasoning is inspectable, not just the answer): $P$ and $Q$ are each individually *unambiguous* — $P\to aP\mid bP\mid\varepsilon$ is right-linear, so for any fixed prefix the production choice at each step is forced by the next character; $Q$'s wrap productions ($aQa\mid aQb\mid bQa\mid bQb$) are likewise forced at each level by the target substring's own current first/last characters, peeling inward. So **all ambiguity comes from the choice of *pivot* — which occurrence of the literal `b` in $S\to PbQ$ splits the string** — and a `b` at position $i$ (0-indexed, string length $n$) is a valid pivot exactly when the suffix after it has **even** length, i.e. $n-i-1$ is even.

Consider the grammar above, and the string **`abababaa`** (length 8).

**(a)** Give a leftmost derivation for `abababaa`. [3 marks]

**(b)** Draw the parse tree corresponding to the derivation you gave in (a). [2 marks]

**(c)** Show that the given grammar is ambiguous by drawing two more parse trees (apart from the one in (b)) for the string in (a). [4 marks]

**(d)** Write two strings of length four that have exactly one parse tree in the given grammar. [1 mark]

**Difficulty:** Medium — the derivation/tree mechanics are routine once the "pivot = any valid `b`" insight lands, but part (c) (finding the *other* valid pivots) is genuinely the hardest single sub-part on the whole exam per the PYQ analysis.
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

---

### Slot 4 — Pushdown Automata

Real anchor (Spring26 Set A, Problem 6, CO3, 10 pts): an "ends with" language (3 pts), a `#`-separated length-ratio language (3 pts), a reversal language with a parity condition (4 pts) — this exact 3-part shape is near-universal across all 3 sittings per the PYQ analysis. New languages below, each construction independently built from the PDA techniques in `CSE331_lecture_15_2026-08-17` and `CSE331_lecture_16_2026-08-19` and verified before posing.

Consider the following languages:

$$L_1 = \{w \in \{0,1\}^* : w \text{ ends with } 10\}$$
$$L_2 = \{w_1\#w_2 : w_1, w_2 \in \{0,1\}^*, \ 2|w_1| = |w_2|\}$$
$$L_3 = \{w\#w^R : w \in \{0,1\}^* \text{ and } w \text{ contains an even number of 1s}\}$$

**(a)** Give the state diagram of a PDA that recognizes $L_1$. [3 marks]

**(b)** Give the state diagram of a PDA that recognizes $L_2$. [3 marks]

**(c)** Give the state diagram of a PDA that recognizes $L_3$. [4 marks]

**Difficulty:** Medium — (a) barely needs the stack at all (a 3-state nondeterministic guess suffices, same as Lecture 16's "cautionary example" about regular languages); (b) is the standard push-2-per-symbol length-ratio technique; (c) is Hard — it combines the `#`-gated reversal-match technique with a state-based parity tracker gating entry to the matching phase.
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

---

### Slot 5 — Turing Machine

Real anchor (Spring26 Set A, Problem 3, CO4, 10 pts): a TM-configuration read-off (2 pts), two TM-design sub-questions (3+3 pts), and a DFA-property TM built as $A_{DFA}$ plus one extra finite check (2 pts, already worked as `CSE331_decidability_reducibility_pyq_practice.md` Section C1). Configuration and languages below are new but built in the same style, each solved and checked before posing.

**(a)** Given the Turing machine configuration $110\,q_5\,001$, identify the tape contents, the current state, and the current head position. [2 marks]

Consider the following languages:

$$L_1 = \{w \in \{0,1\}^* : \text{every } 1 \text{ in } w \text{ is followed by at least one } 0 \text{ somewhere later in } w\}$$
$$L_2 = \{w \in \{0,1\}^* : w \text{ contains an equal number of } 0\text{s and } 1\text{s}\}$$
$$L_3 = \{\langle D, w\rangle : D \text{ is a DFA that accepts } w \text{ and } w \text{ contains an odd number of } 0\text{s}\}$$

**(b)** Give the state diagram of a Turing machine that decides $L_1$. [3 marks]

**(c)** Give the state diagram of a Turing machine that decides $L_2$. [3 marks]

**(d)** Give a Turing machine that decides $L_3$. [2 marks]

**Difficulty:** Medium — (a) and (d) are Easy (pure notation-reading and direct $A_{DFA}$-plus-one-check reuse); (b) hides a genuine insight (the condition collapses to "the string doesn't end in 1" — a common trap if attacked by brute construction instead of first finding the simpler equivalent characterization); (c) is Hard, since the crossing-off technique needs the *interleaved*-matching variant (restart-to-tape-start per round), not the ordered-block variant taught for $0^n1^n$.
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

---

### Slot 6 — True/False: CFL / Non-Regular Language Properties

Real anchor: **two** separate real T/F sections exist for this slot family — Spring26 Set A Problem 5 (CO3, 5 pts: containment hierarchy, pumping-lemma edge cases, recognizable-halting, PDA power, ambiguity) and Fall25 Set A Problem 3 (CO3, 5 pts: PDA determinism, acceptance convention, regular-vs-CFL containment, CFG syntax validity, CFL closure). Both read in full from the PDFs. 10 new statements below, same format ("write True/False, and the correct statement if False"), each testing the identical underlying misconception as its real counterpart — independently verified against `CSE331_lecture_12` through `18`.

**(a)** Decidable language $\subset$ recognizable language $\subset$ context-free language $\subset$ regular language.

**(b)** Every language that satisfies the pumping lemma's condition is regular.

**(c)** For Turing-decidable languages, the machine may loop forever on some inputs.

**(d)** A PDA can recognize any language that a Turing machine can recognize.

**(e)** A context-free grammar for a non-regular language is always ambiguous.

**(f)** Every context-free language can be recognized by some *deterministic* pushdown automaton.

**(g)** If a PDA reaches a non-accepting state after reading the entire string but the stack is empty, the string is still accepted.

**(h)** All regular languages are non-context-free.

**(i)** "$S \to \varepsilon \mid A$" is not a valid CFG production.

**(j)** The set of context-free languages is closed under intersection.

[1 mark each, 10 marks total]

**Difficulty:** Medium — each statement targets a single, specific misconception straight from the course's core theorems (containment hierarchy, PDA acceptance convention, CFL closure properties), not an obscure edge case; the plausibility of the *wrong* half of each pair is what makes them worth timing yourself on.
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

---

### Slot 7 — Decidability and The Halting Problem

Two already-solved real PYQ anchors exist in `CSE331_decidability_reducibility_pyq_practice.md`: Section B1 (prove $L_1 \cdot L_2$ decidable given $L_1, L_2$ decidable — Fall25 Problem 6e, 2 pts) and Section C2 (the pigeonhole DFA-property proof — Spring26's actual 5-pt bonus, Problem 7). Order-preserving relabeling of each below — same proof *technique*, different target claim, independently re-derived and checked (not the same claim with cosmetic changes, since both source proofs are about arbitrary/structural objects, not concrete numbers to relabel).

**(a)** Let $L$ be a decidable language. Prove that $L^* = \{x_1x_2\cdots x_k : k \geq 0, \text{ each } x_i \in L\}$ is also decidable.

*(Same "check every possible split point" technique as the concatenation proof — generalized from exactly 2 pieces to any number of pieces via a dynamic-programming-style reachability argument: `reachable[0] = true`; `reachable[i] = true` iff some `reachable[j] = true` (j < i) with `w[j+1..i] ∈ L`; accept iff `reachable[n] = true`. Halts: $O(n^2)$ decider-runs, each finite. Correct: by induction, `reachable[i]` holds iff `w[1..i]` splits into zero or more pieces each in $L$.)*

**(b)** Let $D$ be a DFA with $m$ states. Prove that the language $L = \{\langle D \rangle : D \text{ accepts } 0^k \text{ for every } k \geq 0\}$ is decidable.

*(Same pigeonhole/bounded-path technique as Spring26's bonus — but existential flips to universal. Feed $D$ the string $0^k$ for $k=0,1,\dots,m$ ($m{+}1$ steps along the single deterministic all-0s path); by pigeonhole some state repeats within this window, so the window already contains every state the path will ever visit. Accept iff all $m{+}1$ visited states are accepting — necessary since $0^0,\dots,0^m$ must each individually be accepted, sufficient since the eventual cycle only revisits states already confirmed accepting.)*

**Difficulty:** (a) Medium-Hard — same finite-splits idea as the real PYQ, but the "any number of pieces" generalization needs the DP-recursion wrinkle, not just a direct restatement. (b) Hard — matches the real bonus's own standing as the hardest single item on the paper.
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

---

### Slot 8 — Reducibility

**No PYQ anchor exists** — confirmed zero precedent across all 6 collected papers (`CSE331_decidability_reducibility_pyq_practice.md`'s own note: "No past Final has included a full reduction proof... confirmed across all 6 collected papers"). Built as a fresh, fully-solved derivation following the exact reduction discipline from `CSE331_lecture_19_2026-09-02` ("does the conversion halt? does it preserve the answer?"), reusing the *same* $M'$-construction move as the lecture's own $E_{TM}$ proof — this is the standard textbook target for the technique once $E_{TM}$ is understood.

**(a)** Let $\text{INFINITE}_{TM} = \{\langle M \rangle : L(M) \text{ is infinite}\}$. Prove that $\text{INFINITE}_{TM}$ is undecidable.

**Difficulty:** Hard — no PYQ precedent, and correctly identifying which infinite/finite pair ($\Sigma^*$ vs. $\varnothing$) the reduction should target is the actual difficulty, not the mechanics once that's seen.
**Status:** drafted, not yet posed.
**Answer:** _(pending)_

<details>
<summary>Verified reference solution for Slot 8(a) — do not open before attempting</summary>

**Proof (reduction from $A_{TM}$).** Assume for contradiction that $\text{INFINITE}_{TM}$ is decidable, with decider $R$.

Given an arbitrary instance $\langle M, w \rangle$ of $A_{TM}$, construct $M'$ that, on any input $x$ (ignoring $x$): runs $M$ on $w$; if $M$ accepts $w$, $M'$ accepts $x$; if $M$ rejects $w$ or loops forever on $w$, $M'$ correspondingly rejects or loops on $x$.

Constructing $\langle M' \rangle$ always halts — it is simply a fixed description built from $M$ and $w$.

*Case 1 ($M$ accepts $w$):* $M'$ accepts every input $x$ → $L(M') = \Sigma^*$, which is infinite.
*Case 2 ($M$ does not accept $w$):* $M'$ never accepts any input $x$ → $L(M') = \varnothing$, which is finite.

So $L(M')$ is infinite $\iff$ $M$ accepts $w$.

Run $R$ on $\langle M' \rangle$: if $R$ accepts (says $L(M')$ is infinite) → conclude $M$ accepts $w$ (accept for $A_{TM}$). If $R$ rejects (says $L(M')$ is finite) → conclude $M$ does not accept $w$ (reject for $A_{TM}$).

This procedure would decide $A_{TM}$ — contradicting its undecidability (Lecture 18). So $R$ cannot exist: $\text{INFINITE}_{TM}$ is not decidable. $\blacksquare$

</details>

---

## After completion

Fold the finished set + verified answers into this course's revision guide, if one exists (`project_revision_guide_workflow` memory), as a "Mock Final" section. Also cross-reference [CSE331_final_probable_questions](../pyq/CSE331_final_probable_questions.md)'s "Revision Priority" section once done, to confirm nothing in the ranked probable-questions list was left unpracticed.
