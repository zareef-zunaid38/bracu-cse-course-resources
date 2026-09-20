---
course: CSE331
type: practice-questions
source: In-class example, reported by classmate (last class before Mid-Term, 2026-07-22)
status: reviewed
---

# CSE331 — Proof: Every NFA Can Have At Most One Accepting State

> Shown in class as an example proof ahead of the Mid-Term. Faculty indicated the Mid-Term will include proof-based questions this time, and recommended practicing proofs from the textbook rather than relying on in-class examples alone. Not independently confirmed against the original lecture — transcribed from a classmate's photographed notes (2026-07-22).

---

## Claim

Prove that every NFA can be converted to an equivalent NFA with at most one accepting state.

## Proof (by cases on the number of accepting states in the original NFA)

**Case 1 — No accepting states.** 0 ≤ 1, holds trivially.

**Case 2 — Exactly one accepting state.** 1 ≤ 1, holds trivially.

**Case 3 — More than one accepting state.** Construct a new state. Add an ε-transition from each original accepting state to this new state. Make the new state the only accepting state, and mark all original accepting states as non-accepting.

Because ε-transitions consume no input, any string that reached an old accepting state can still reach acceptance — it now continues via ε into the new joint accepting state. The resulting NFA is equivalent (accepts the same language) and has exactly one accepting state. ∎

**Diagram (from the shown example):** a small NFA with two states, illustrating the construction — an existing accepting state gets an ε-transition added into a new state, and the new state becomes the sole accepting state while the old one is marked non-accepting.

---

## Important clarification — do not overgeneralize this proof

This proof does **not** mean an NFA's formal definition restricts it to one accepting state. Multiple accepting states are entirely valid in an NFA by definition. This proof only shows that any NFA *can be converted* into an equivalent NFA with a single accepting state, via the ε-transition construction in Case 3 — it is a closure/normalization result, not a structural restriction on NFAs in general.

Confirmed with multiple classmates after the lecture, per report 2026-07-22 — this is a known point of confusion with this particular proof.
