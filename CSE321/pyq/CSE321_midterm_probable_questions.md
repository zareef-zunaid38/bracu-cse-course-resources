---
type: pyq-analysis
course: CSE321
exam: midterm
status: complete
date: 2026-07-29
---

# CSE321 — Midterm Probable Questions & PYQ Pattern Analysis

**Exam:** 2026-07-30, 2:00–3:30 PM, Room 07B-13C (Sec 2) | **Weight:** 25% | **Created by:** Central (BRACU question committee) — not NTR-authored, see [[cse321_exam_notes]].

**Confirmed syllabus (Topics 1–19):** Process → Threads → CPU Scheduling → Process Synchronization (deadlock/RAG included under Synchronization). Full topic table in [[cse321_topics]].

**Sources reviewed:** All available midterm PYQs on HDD (`Course-Library/cse/cse321/pyq/`) — Spring 2022, Summer 2022, Fall 2022 (A/B), Spring 2023 (A/B), Summer 2023 (A/B), Spring 2024 (A/B), Fall 2024 (A/B), Spring 2025 (A/B), Summer 2025 (A/B), Fall 2025 (A/B), Spring 2026 (A/B) — 13 sittings, 20 question sets total.

---

## Direct Answer: Did the Questions Change, or Follow a Pattern?

**Both — the topical skeleton is stable, but format and difficulty have drifted meaningfully.** Two fixtures have appeared in every single one of the 13 sittings with no exception:

1. A `fork()` output-tracing question (Process)
2. A Gantt-chart CPU scheduling calculation with average waiting/turnaround time (CPU Scheduling)

Everything else has moved:

- **Synchronization was absent from the midterm entirely from Spring 2022 through Summer 2023** (7 sittings, 0%). It entered the scope at Spring 2024, disappeared again at Fall 2024, then has been present in **every sitting since** (Spring 2025 → Spring 2026, 4/4). Treat it as a firmly-established recent-era fixture, not a historical constant — don't under-revise it because older PYQs don't feature it.
- **CPU Scheduling escalated from plain RR/SRTF/Priority (2022–2024) to Multilevel Feedback Queue — MLFQ — Gantt charts in every sitting from Spring 2025 onward (4/4).** This is a real difficulty increase: multiple queues, I/O-triggered promotion/demotion, and quantum bookkeeping across levels, not a single-algorithm trace.
- **Question style shifted from direct textbook phrasing (2022–2023) to named-scenario wrapping (2024 onward)** — "Dipu is implementing...", "Adhara wants to build..." — same underlying mechanics, dressed in a story.
- **Duration crept upward:** 70 min (2022) → 75 → 80 (2024) → 90 min, stable for the last 4 sittings (Spring 2025–Spring 2026).
- **Full marks fluctuate** (25 or 30, no fixed rule) — currently 25 as of the most recent sitting (Spring 2026).
- **Question count shrank while content deepened** — fewer discrete questions, more heavily subdivided (i/ii/iii nesting) from 2025 onward.

**Verdict:** not a copy-paste exam year to year, but not unrecognizable either. Anchor revision on the two universal fixtures and the four-module coverage; treat the specific sub-skill tested within each module (which algorithm, which synchronization mechanism) as the part that shifts.

---

## Marks Distribution by Topic (Approximate, Across Eras)

| Era | Sittings | Process | Threads | CPU Scheduling | Synchronization |
|---|---|---|---|---|---|
| Sp22–Su23 | 7 sittings | ~35% | ~20–25% | ~40% | **0%** |
| Sp24 | 2 sets | ~27% | ~17% | ~23% | ~33% |
| Fa24 | 2 sets | ~40% | 0% | ~60% | 0% (outlier) |
| Sp25–Sp26 | 8 sets | ~25–30% | ~15–20% | ~30–35% | ~20–25% |

Not stable — weight it toward the most recent era (Sp25–Sp26) as the best predictor for tomorrow, not the historical average.

---

## Question-Type Pattern per Module

**Process** — always a `fork()` output-tracing question, increasingly wrapped in a named scenario (client-server, NASA rover, media app) rather than bare code since ~2024. Zombie/orphan process concept is a genuinely new recurring item, first appearing Summer 2025 and present in 3 of the last 4–5 sittings.

**Threads** — stays conceptual/scenario-based: parallelism type identification, Amdahl's-Law speedup calculation (constant 2022–2024, **absent since Fall 2024** — possibly retired), multithreading-model identification (2023–2024 fixture, now largely superseded), thread pool vs. dynamic creation tradeoffs. Rarely a full pthread code-trace. When threads and process-count merge (Sp25/Su25), it's usually "how many processes/threads get created" arithmetic, not execution-order tracing.

**CPU Scheduling** — always a Gantt-chart numeric question. **Expect MLFQ, not plain RR/SRTF/Priority**, given 4/4 recent sittings.

**Synchronization (when present)** — never asks for the three classical problems (bounded buffer / readers-writers / dining philosophers) by name. Instead: real-world scenarios (parking lot, restaurant tables, library pods, bank vault, train-ticket booking) mapped onto counting semaphore / mutex / TAS / CAS mechanics, plus "find the bug" in a TAS/CAS/semaphore implementation, plus multi-thread semaphore-guarded output-trace tables. The exact format is still evolving: Sp24 favored mutex-table completion → Sp25–Fa25 favored output-trace tables → **Sp26 dropped semaphore code entirely in favor of CAS-based reasoning and select-all MCQs** (the most recent format).

**Resource Allocation Graph / deadlock conditions — zero appearances across all 13 sittings**, despite being nominally in-scope. Likely historically a final-exam-only topic in practice. Don't over-invest here relative to the fixtures above, but don't assume it's impossible — 13 sittings isn't proof of never.

---

## Set A vs. Set B Pattern

**Every semester with two sets is 100% structurally parallel** — identical question slots, identical mark allocations, identical scenario types, with only numbers, character names, or surface framing swapped (e.g. Fa22: RR quantum 11 vs. 12; Sp24: "checkout key" vs. "bank teller token" for the same mutex concept; Sp26: `sunny()` vs. `rainy()` with mirrored `fork()==0` branches). Practicing one set is direct practice for the paired set's *identical skill*.

---

## Probable Questions for Tomorrow — Ranked

<!-- Ranking rationale: frequency across 13 sittings, weighted toward the most recent era (Sp25–Sp26), which best predicts the committee's current format. -->

**Near-certain — appeared in every sitting reviewed:**

1. **`fork()` multi-branch output trace**, likely combined with `wait()` and shared/unshared variable state, probably scenario-wrapped. This is the single most reliable fixture on this exam — do not walk in without having hand-traced several of these tonight.
2. **Gantt-chart CPU scheduling calculation** — expect **MLFQ** specifically (multiple queues, quantum per level, I/O-triggered priority changes), not a single plain algorithm. Compute avg waiting/turnaround/response time.

**High probability — standard in the recent era (last 4 sittings):**

3. **Synchronization scenario question** — semaphore/mutex applied to a real-world analogy (parking lot, restaurant, bank vault style). Present in 4/4 of the last four sittings.
4. **TAS/CAS "find the bug" or CAS-based concurrent-increment reasoning** — this is the Spring 2026 (most recent) format; may repeat or may already have rotated to something else, but CAS mechanics specifically are worth being solid on given the trend away from raw semaphore code.
5. **IPC method selection** (shared memory vs. message passing/pipes/sockets, storage-vs-speed tradeoff reasoning) — present in 3 of the last 4 sittings. Directly matches the Quiz 1 IPC scenario question you already saw (see [[cse321_exam_notes]] Quiz 1 pattern).
6. **Process/thread creation counting** from mixed `fork()` + `pthread_create()` code — "how many processes/threads exist at this point" arithmetic.

**Moderate probability:**

7. **Zombie/orphan process concept** — emerging since Summer 2025, worth a dedicated pass since your module note (Topic 3) already covers it but it's newer than the note's original build date suggests it was weighted for.
8. **Thread pool vs. dynamic thread creation** — efficiency/tradeoff comparison.
9. **Priority scheduling starvation + aging fix.**

**Lower probability but still syllabus-legal — don't skip entirely:**

10. **Amdahl's Law speedup calculation** — dormant since Fall 2024, could return.
11. **Multithreading-model comparison** (many-to-one / one-to-one / many-to-many) — dormant since Spring 2024.
12. **Deadlock / Resource Allocation Graph** — zero precedent across all 13 sittings reviewed; lowest-probability in-scope item. Know the 4 conditions and RAG cycle-detection rule well enough to not lose an easy mark if it does appear, but don't over-allocate revision time here relative to items 1–9.

---

## Revision Priority Implied by This Analysis

Given the self-review flag already on `tracking/tasks-active.md` #3 (Threads and Synchronization content written under the medical-emergency exception, not yet personally studied) — items 3, 4, and 7 above sit exactly in that flagged gap. Treat those as first-pass learning tonight, not review, consistent with the standing flag.
