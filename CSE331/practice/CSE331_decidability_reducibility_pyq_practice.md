---
course: CSE331
type: practice-questions
source: "Actual CSE331 Final PYQs (Spring25, Fall25, Spring26 — Set A+B each, centrally created). Compiled and solved 2026-09-11 as a study aid for Lecture 18–19 (Decidability, Reducibility) — see [[CSE331_lecture_18_2026-08-31]] and [[CSE331_lecture_19_2026-09-02]]. Full PYQ pattern analysis: [[CSE331_final_probable_questions]]."
status: solved
---

# CSE331 — Decidability & Reducibility PYQ Practice (Lectures 18–19)

Every CO4 (Turing Machine/Decidability) question that has actually appeared on a past Final, organized by which technique from Lecture 18–19 it tests, with full worked solutions. Two question types from the past papers are **not** included here on purpose: plain TM-design questions (build a TM deciding language X — that's Lecture 16/17's TM-construction skill, already the subject of Assignment 2B Q2) and the TM-configuration read-off question (already covered with a worked example in [[CSE331_lecture_16_2026-08-19]]).

No past Final has included a full reduction proof ($A_{TM}$/$E_{TM}$-style) — confirmed across all 6 collected papers. Nothing to practice from real PYQs there; the technique itself is in [[CSE331_lecture_19_2026-09-02]].

---

## Section A — Decider/recognizer conceptual questions

These test the piece-1 distinction (decider = always halts; recognizer = may loop) and the "decidable = recognizable + co-recognizable" theorem. All are True/False, "give the correct statement if False."

**A1 (Spring25, Problem 5a, 1 pt).** For any Turing-recognizable language $L$, is it possible to build a recognizer for $\overline{L}$?

*Answer:* **No, not in general.** If a recognizer for $\overline{L}$ always existed, then both $L$ and $\overline{L}$ would be Turing-recognizable — and by the theorem in Lecture 19 ("decidable = recognizable AND co-recognizable"), that would make $L$ decidable. But not every recognizable language is decidable ($A_{TM}$ is the standing counterexample: recognizable, proven undecidable in Lecture 18). So a recognizer for the complement is not guaranteed to exist.

**A2 (Fall25, Problem 6a, 1 pt).** True/False: "Any subset of a decidable set is decidable."

*Answer:* **False.** Decidability is a property of a language's own decider — it says nothing about that language's subsets. Concrete counterexample: $\Sigma^*$ (the language of "accept everything") is trivially decidable. But $A_{TM} \subseteq \Sigma^*$ (loosely, every encoded $\langle M, w\rangle$ pair is some string in $\Sigma^*$), and $A_{TM}$ is undecidable. So a decidable language can have undecidable subsets. Correct statement: "Not every subset of a decidable language is decidable."

**A3 (Fall25, Problem 6b, 1 pt).** True/False: "Let $L = \{a^nb^{4n}c^n : n>0\}$. $L$ is Turing decidable but not recognizable." *(Set B: $\{a^nb^nc^{3n}\}$ — same structure.)*

*Answer:* **False**, and it's self-contradictory independent of what $L$ actually is: the containment hierarchy (Lecture 18) puts Decidable strictly *inside* Recognizable — every decidable language is automatically recognizable (its own decider already works as a recognizer). So "decidable but not recognizable" describes an empty set of languages; no such $L$ can exist. This specific $L$ *is* decidable in the ordinary sense (it's a counting-relation language, decidable by the same crossing-off technique as Lecture 17's $0^i1^j2^k$ constructions). Correct statement: "$L$ is Turing decidable" (full stop — drop the false second half).

**A4 (Spring26, Problem 5a, 1 pt — tagged CO3, but directly tests Lecture 18's hierarchy).** True/False: "Regular language $\subset$ context-free language $\subset$ recognizable language $\subset$ decidable language."

*Answer:* **False.** The last two are swapped. Lecture 18's confirmed containment hierarchy is Regular $\subset$ CFL $\subset$ **Decidable** $\subset$ **Recognizable** — decidable sits strictly inside recognizable, not the other way around. Correct statement: "Regular $\subset$ CFL $\subset$ decidable $\subset$ recognizable."

**A5 (Spring26, Problem 5c, 1 pt).** True/False: "For recognizable languages, the Turing machine halts on every input."

*Answer:* **False.** This is exactly piece 1's distinction. A recognizer for $L$ is only guaranteed to halt-and-accept when the input *is* in $L$. On an input *not* in $L$, it may reject (halt) — or it may loop forever. Only a **decider** guarantees halting on every input, and decidable is a strictly smaller class than recognizable. Correct statement: "For decidable languages, the Turing machine halts on every input" (recognizable alone doesn't give you that guarantee).

---

## Section B — Decidable-language closure proofs

Tests the "build a decider using given deciders as subroutines" pattern — the same move as the Union proof now in [[CSE331_lecture_18_2026-08-31]], applied to a different set operation.

**B1 (Fall25, Problem 6e, 2 pts).** Let $L_1$ and $L_2$ be decidable languages over the same alphabet $\Sigma$. Prove that $L_1 \cdot L_2 = \{xy : x \in L_1, y \in L_2\}$ is also decidable.

*This is harder than the union proof* — with union, you run $T_1$ and $T_2$ on the *same* input $w$ and combine the outcomes. With concatenation, $w$ has to be **split** into two pieces $x$ and $y$, and you don't know in advance where the split falls (if it exists at all). The fix: since $w$ is finite, there are only finitely many places it *could* split — try all of them.

**Proof.** Let $T_1, T_2$ be deciders for $L_1, L_2$. Construct $T$ for $L_1 \cdot L_2$. On input $w$ of length $n$:
1. For each way of splitting $w$ into a prefix $x$ and suffix $y$ (i.e. $x$ = first $i$ characters, $y$ = the remaining $n-i$ characters, for every $i = 0, 1, \dots, n$ — there are exactly $n+1$ such splits):
   a. Run $T_1$ on $x$ and $T_2$ on $y$.
   b. If both accept, **accept** $w$ and stop.
2. If none of the $n+1$ splits made both $T_1$ and $T_2$ accept, **reject**.

*Halts:* there are only $n+1$ splits (finite, since $w$ is finite), and each split costs two decider-runs (each guaranteed to halt, by the same "no loop possible" guarantee as $A_{DFA}$). A finite number of finite computations is itself finite — $T$ always halts.

*Correct:* $w \in L_1 \cdot L_2 \iff$ some split $w = xy$ has $x \in L_1$ and $y \in L_2$ $\iff$ some split makes both $T_1$ and $T_2$ accept $\iff$ step 1 finds it and $T$ accepts. Matches exactly.

**Worked trace**, reusing $T_1$ = "even number of 0s", $T_2$ = "contains at least one 1" (same pair as the Union proof's trace):

Take $w = 010$ ($n=3$). Try $i=0$: $x = \varepsilon$, $y = 010$. $x \in L_1$? Zero 0s is even → yes. $y \in L_2$? Contains a 1 → yes. **Both accept on the first split tried** → $T$ accepts $010$.

Take $w = 00$ ($n=2$), a reject case — check *all three* splits: $i=0$: $x=\varepsilon$ (even 0s ✓), $y=00$ (no 1 ✗) — fails. $i=1$: $x=0$ (one 0, odd ✗) — fails already, no need to check $y$. $i=2$: $x=00$ (even 0s ✓), $y=\varepsilon$ (no 1 ✗) — fails. All three splits fail → $T$ rejects $00$. Sanity check: $00$ has no `1` anywhere, so no suffix $y$ of it can satisfy "contains a 1" — correctly unsatisfiable.

---

## Section C — "Prove ⟨D⟩ has property X is decidable" (DFA-property proofs)

Tests the $A_{DFA}$/$E_{DFA}$ family: simulate a *given, finite* automaton and read off an answer. The flagged gap — these appeared on real Finals despite $E_{DFA}$ being framed as "not exam-required" in the lecture note.

**C1 (Spring26, Problem 3d, 2 pts).** Give a Turing machine that decides $L_3 = \{\langle D, w\rangle : D \text{ is a DFA that accepts } w \text{ and } w \text{ contains an even number of 1s}\}$.

This is $A_{DFA}$ with one extra condition ANDed in — nothing new mechanically, just two independent finite checks combined.

**Construction.** On input $\langle D, w \rangle$: (1) Simulate $D$ reading $w$ (exactly the $A_{DFA}$ decider) — accept-condition-1 is "simulation ends in an accepting state." (2) Separately scan $w$ once, counting 1s — accept-condition-2 is "count is even." (3) Accept iff both conditions hold; otherwise reject.

*Halts:* step 1 finishes in $|w|$ steps (the $A_{DFA}$ guarantee), step 2 finishes in $|w|$ steps (one linear pass over a finite string) — both bounded, so the whole machine halts. *Correct:* by construction, it accepts exactly when both defining conditions of $L_3$ hold. Decider exists → $L_3$ decidable.

**C2 (Spring26, Problem 7 — bonus, 5 pts, both sets).** Let $L = \{\langle D \rangle : D$ is a DFA such that if any string $w$ is accepted by $D$, it contains at least one 1$\}$. Prove that $L$ is decidable.

This one is genuinely different from C1 — it's not "does $D$ accept *this* $w$," it's a property of $D$ **itself**: does $D$ ever accept *any* string with **zero** 1s (i.e. any string of the form $0^k$)? Naively that looks like infinitely many strings to check ($k = 0, 1, 2, \dots$) — but $D$ has only finitely many states, and that's the key.

**The pigeonhole argument.** Say $D$ has $m$ states. Feed $D$ only $0$s, one at a time, starting from the start state — this traces out exactly one path: $q_0 \to q_1 \to q_2 \to \dots$ (the state reached after reading $0^k$). There are only $m$ distinct states available. So among the first $m+1$ states visited (after reading $0^0, 0^1, \dots, 0^m$), some state *must* repeat — meaning the path has entered a cycle by step $m$ at the latest. Two cases: **(a)** an accepting state was hit somewhere in the first $m+1$ steps — then $D$ *does* accept some all-0 string, so $L$'s property fails for this $D$. **(b)** no accepting state was hit in the first $m+1$ steps — then the path has looped through only non-accepting states, and since it's now cycling forever among the same states, it will *never* reach an accepting state no matter how many more 0s are fed in. So checking the first $m+1$ steps is *conclusive* — this is the same finite-reachability trick as $E_{DFA}$'s reachability check in Lecture 19, just applied along the single "all zeros" path instead of the whole transition graph.

**Proof.** Let $D$ have $m$ states. Construct $M$. On input $\langle D \rangle$:
1. For $k = 0, 1, 2, \dots, m$: simulate $D$ on the string $0^k$.
2. If any of these $m+1$ simulations ends in an accepting state, **reject** $\langle D \rangle$ (found a counterexample string with no 1s that $D$ accepts).
3. If none of them do, **accept** $\langle D \rangle$ (by the pigeonhole argument, no longer all-0 string can newly reach an accepting state either).

*Halts:* $M$ performs exactly $m+1$ bounded DFA-simulations, each guaranteed to finish (same guarantee as $A_{DFA}$) — a fixed finite number of finite computations, so $M$ always halts. *Correct:* directly by the case analysis above. Decider exists → $L$ decidable.

---

## What's deliberately left out (and why)

- **Plain TM-design questions** (Spring25 P5b/P6-bonus, Fall25 P6c/d, Spring26 P3b/c) — these ask you to *build* a TM deciding an arbitrary language (counting relations, comparisons, etc.). That's Lecture 16/17's construction skill, not Lecture 18/19's decidability *theory* — and it's the exact skill Assignment 2B Q2 already drilled. Say the word if you want these solved too, as a separate practice set.
- **TM configuration read-off** (Spring26 P3a) — pure notation-reading, already has a full worked example in Lecture 16 (see that note's Anki card).
- **Full reduction proofs** ($A_{TM}$, $E_{TM}$-style) — zero PYQ instances across all 6 papers to practice from. The technique itself is in Lecture 19; once you've worked through it there, this file can get a Section D once a reduction-style PYQ actually surfaces (or on request, as an invented-but-representative practice problem).
