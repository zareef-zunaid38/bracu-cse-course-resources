---
type: lecture-note
course: CSE331
date: 2026-06-08
topics: [marks-distribution, set-notation, alphabet, string, language]
status: reviewed
source: "Transcribed from Farhan Labib's handwritten notes (scanned PDF, 2026-07-01). Original discarded after user cross-check. Content re-verified 2026-09-05 (definitions and marks breakdown checked against index.md and standard theory) — no errors found."
---

# CSE331 — Lecture 1: Preliminaries

*Date approximate — first pages of the notebook carried no date stamp; placed before the first dated page (2026-06-15). Covers notebook pages 1–5; page 6 onward (Finite Automaton introduction) is filed under [CSE331_lecture_02_2026-06-15](CSE331_lecture_02_2026-06-15.md) since it flows directly into the dated content with no topic break.*

## Marks Distribution

| Component | Weight | Notes |
|---|---|---|
| Quiz | 20% | 4 or 5 quizzes, best 3 counted |
| Assignment | 10% | 2 assignments, each split into Part 1 + Part 2 |
| Mid-Term | 35% | |
| Final | 35% | |
| Attendance | — | No marks |

*(Cross-checked against [cse331_topics](../topics/cse331_topics.md) / `index.md`, which independently records the same breakdown.)*

## Automata: Definitions and Properties

Definitions and properties of mathematical models — Finite Automaton, Context-Free Grammar (CFG).

**Finite automaton — applications:** text processing, compilers, hardware design.

## Set Notation Used Throughout the Course

- $\cup$ — Union
- $\cap$ — Intersection
- $\overline{A}$ — Complement
- $\emptyset$ — Empty set

**Worked example:**
$$L_1 = \{a, b\}, \quad L_2 = \{b, c\}$$
$$L_1 \cup L_2 = \{a, b, c\} = U$$
$$L_1 \cap L_2 = \{b\}$$
$$\overline{L_1} = \{c\}$$

Membership: $a \in L_1$, $b \in L_1$, $c \notin L_1$.

**Note:** empty set and null mean the same thing for *sets*, but not for *strings* (see below).

**Cartesian product example:**
$$L_1, L_2 = \{a, b\}, \{b, c\}$$
$$L_1 \times L_2 = \{(a,b), (a,c), (b,b), (b,c)\}$$

## Alphabet

**Definition:** a non-empty, finite set. Elements are called *symbols*.

$$\Sigma_1 = \{0, 1\}, \quad \Sigma_2 = \{a, b\}$$

Example alphabet: $\{a, b\}$, string example: `01a` (illustrating symbols drawn from an alphabet).

## String

**Definition:** a combination of symbols from the same alphabet.

Example: $w = abab$, $|w| = 4$ (length notation).

- $\emptyset$ (null) as a **string** → invalid object.
- The **empty string** → a valid string whose length is 0. Denoted $\varepsilon$ (epsilon). $|\varepsilon| = 0$.

All strings over $\{0,1\}$ up to length 2: $\varepsilon, 0, 1, 00, 01, 10, 11$ — maximum length 2, alphabet $\{0, 1\}$.

$w^R$ → reverse of $w$. Example: $w = abab \Rightarrow$ reverse $= baba$.

## Substring vs. Subsequence

- **Substring:** consecutive symbols from any part of the string.
- **Subsequence:** symbols in chronological order as they appear in the string, not necessarily consecutive.

## Language

**Definition:** for a particular alphabet, the rules that must be followed to generate a string.

Example: the reference set $\{\varepsilon, 0, 1, 00, 01, 10, 11\}$ (all strings over $\{0,1\}$ up to length 2), annotated with "even number of 0s" as an example restriction defining a language — i.e., the actual language is the *subset* of that list satisfying the rule ($\varepsilon, 00, 11, \dots$), not the full list itself.

## Anki Cards

START
Basic
In CSE331, what is the difference between the empty set (∅) as a set vs. as a string?
Back: As a set, ∅ is valid (the set with no elements). As a string, ∅ is an invalid object — the valid "no-content" string is ε (epsilon), which has length 0.
END

START
Basic
What is the difference between a substring and a subsequence?
Back: A substring is made of consecutive symbols from the string. A subsequence preserves chronological order but does not require consecutive symbols.
END
