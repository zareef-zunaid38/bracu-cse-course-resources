---
type: pyq-analysis
course: CSE321
exam: final
status: complete
date: 2026-08-01
---

# CSE321 — Final Probable Questions & PYQ Pattern Analysis

**Exam:** 2026-09-17, 2:00–4:00 PM | **Weight:** 35% | **Created by:** Central (BRACU question committee) — not NTR-authored, see [cse321_exam_notes](../cse321_exam_notes.md).

**Confirmed syllabus (Topics 20–32):** File Systems → Memory Management → Protection → Security. Full topic table in [cse321_topics](../topics/cse321_topics.md). Covers the same [Module 5](../concepts/CSE321_m5_file_systems.md), [Module 6](../concepts/CSE321_m6_memory_management.md), [Module 7](../concepts/CSE321_m7_protection.md), [Module 8](../concepts/CSE321_m8_security.md) notes.

**Sources reviewed:** 14 Final sittings exist (`CSE321/pyq/`), 23 files total (most have Set A/B). **8 of the 14 sittings were read in full detail** — all 4 most recent (Spring 2026, Summer 2025, Fall 2025, Spring 2025 — both sets each) plus 4 chronological anchor points spanning the rest of the timeline (Fall 2024, Summer 2024, Summer 2023, Spring 2022). The remaining 6 (Fall 2022, Spring 2023, Spring 2024, Summer 2022, Summer 2019, Spring 2019-makeup) were not individually reviewed — the pattern within each era proved consistent enough across the sampled sittings that a full 23-file read was judged unnecessary. Treat unreviewed mid/old sittings as *probably* consistent with their era's pattern below, not confirmed.

---

## Direct Answer: Did the Questions Change, or Follow a Pattern?

**The exam has changed shape substantially over time — more than the Midterm has.** This isn't a stable-skeleton-with-drifting-details situation like the Midterm; it's closer to **two different exams** separated by an era boundary around Spring 2025.

**Pre-Spring-2025 era (Spring 2022 through Summer 2024, sampled: Sp22, Su23, Fa24, Su24):**
- **Zero File Systems questions** in any sampled sitting from this era.
- Heavy **Synchronization/Deadlock** content: Resource Allocation Graph construction, **Banker's Algorithm** (safe-state analysis, safety sequence, request-granting) — a topic that does **not appear anywhere in the current topic map at all**, yet was a major, multi-part, high-mark component in Sp22 and (implicitly, by era) likely Su22/Sp23/Su23.
- Heavy **Contiguous Allocation** testing: first-fit/best-fit/worst-fit partition placement + external fragmentation, in every sampled sitting from this era (Sp22, Su23, Fa24, Su24) — this is the *exact* topic the current theory slide deck completely omits (see [CSE321_m6_memory_management](../concepts/CSE321_m6_memory_management.md)'s slide-verification note on Topic 25).
- **Peterson's Solution timeline problems** (detailed instruction-by-instruction execution tables with context-switch timing) — a substantial, recurring essay-style question in Fa24 and Su23.
- Pre-midterm topics (fork, CPU scheduling, synchronization) retested heavily and in full essay depth, not just small identification sub-questions.

**Post-Spring-2025 era (Spring 2025 onward, all 4 most recent sittings — Sp25, Su25, Fa25, Sp26):**
- **File Systems became a fixture** — present in **4/4** recent sittings, usually the largest or second-largest section (12–18 of 35–50 marks). This is a genuinely new emphasis, not present at all before Sp25.
- **Contiguous allocation and Banker's Algorithm/RAG both disappeared entirely** — 0/4 recent sittings test either. Don't over-invest in either despite their historical weight; they read as retired topics under the current committee, though "0/4" isn't proof of "never again," same caveat as the Midterm analysis's RAG finding.
- **Memory Management stayed the largest section throughout both eras** — the one true constant across all 8 sampled sittings.
- **Protection appears in 2/4 recent sittings** (Fa25, and partially Sp25 via an access-matrix question), always small (2–5 marks), always a single applied scenario (access-matrix right application, or a "domain is static, how do you add rights" scenario) — never a deep-recall question about revocation schemes or POSIX capability bitmaps.
- **Security appears in only 1/4 recent sittings** (Fa25 only — both sets), small (5 marks total), always: (a) a buffer-overflow-vulnerability code-spotting question, (b) a security-violation-*method* identification (masquerading/replay/session-hijacking style, not the violation-*category* list). **Cryptography, firewalls, and IDS/IPS have never appeared** in any sitting sampled, recent or old — consistent with the slide deck itself omitting cryptography entirely (see [CSE321_m8_security](../concepts/CSE321_m8_security.md)'s slide-verification note).
- **Pre-midterm topics reappear, but lighter** — small (1–2 mark) identification sub-questions folded into a Process/Sync-labeled Q1/Q2 (e.g. "which PCB field," "identify the synchronization bug," a short output-trace), not the full essay-style Peterson's-timeline problems of the older era.
- Question style: named scenarios throughout ("Shoumo implemented...", "Dipu is designing...", "Kabbya executes..."), consistent with the Midterm's own drift toward scenario-wrapping.
- Difficulty tagging (CA-E / CA-M / CA-D) appears explicitly on questions from Fa25 onward — a recent addition worth knowing the abbreviations for (Easy/Medium/Difficult), though the tagging itself doesn't change what to study.

**Verdict:** anchor revision almost entirely on the post-Sp25 era. The pre-2025 pattern (Banker's, contiguous allocation, deep Peterson's timelines) reads as a genuinely different exam design that the committee has moved away from — useful for recognizing question *style* evolution, but low-value as a source of specific topics to drill.

---

## Marks Distribution by Topic (Approximate, Across Eras)

| Era | Sittings sampled | File Systems | Memory Mgmt | Protection | Security | Contiguous Alloc. / Deadlock / Banker's | Pre-Midterm Review |
|---|---|---|---|---|---|---|---|
| Sp22–Su24 (pre-2025) | 4 sampled | **0%** | ~45–55% | 0% | 0% | ~25–35% | ~15–25% |
| Sp25–Sp26 (current) | 4 sittings (8 sets) | ~30–40% | ~35–45% | 0–10% | 0–15% | **0%** | ~5–15% |

Weight almost entirely toward the current era (Sp25–Sp26) — it is the best predictor by a wide margin, given how sharply the format broke from the pre-2025 pattern.

---

## Question-Type Pattern per Module (Current Era)

**File Systems (Topics 20–23)** — the most consistent, calculation-heavy section, present in all 4 recent sittings. Recurring exact question types, matching what's now in [CSE321_m5_file_systems](../concepts/CSE321_m5_file_systems.md):
- **Inode max-file-size calculation**, often with an unknown pointer-count to solve *for* (e.g. "find the number of double-indirect blocks given the max file size") rather than a straightforward forward calculation — a harder variant than a first pass might assume.
- **Inode byte/sector address calculation** — near-universal, appeared in every File Systems section sampled (Sp26, Fa25, Sp25). Always phrased with a specific inode size, block size, sector size, and a list of reserved leading blocks (superblock, journal, bitmaps, etc.).
- **Crash-consistency deduction** — given a partial-write crash scenario (which of 2–3 writes succeeded/failed), deduce the resulting inconsistency and its symptom (e.g. "what does `cat` show," "what happens to disk space over time").
- **File access path timeline** — "illustrate the file access path" for a read or write scenario, exactly the skill added to Module 5 in this session's audit patch.
- **Mini file-system design** (Sp26 specifically) — given a scenario with total capacity/file-count constraints, derive block counts and bitmap sizes for each file-system region. A more applied/synthesis-style variant of the standard calculations, worth flagging as a possible escalation in difficulty.
- Small conceptual sub-questions on directory entries, hard links, and journaling write-order tradeoffs round out the section.

**Memory Management (Topics 24–28)** — the largest section in every sitting sampled, both eras. Current-era fixtures:
- **TLB Effective Access Time calculation** — appeared in **every single sitting sampled**, no exception, across both eras. The single most reliable fixture on this entire exam. Variants ask to solve for any one of hit ratio, TLB access time, or memory access time given the other two plus EAT — not just the standard "compute EAT" direction.
- **Multi-level paging derivation** — 2-level or 3-level, "illustrate the logical address space... until the outermost page table can be allocated in a frame," present in nearly every recent sitting. This is exactly the Key Skill (3) added to Module 6 in this session's audit patch — the module's densest and most heavily tested individual skill.
- **Page-table-size calculation** (single-level) — given logical address bits, page size, RAM size, and per-entry overhead bits, compute total page-table size for a process using max available memory.
- **Page replacement trace** — FIFO/LRU/Optimal, sometimes with a **deliberately "buggy" or "sub-optimal" variant algorithm** the question defines (e.g. "when a page is replaced, the next-frame page is also replaced," or "the page needed soonest is replaced instead") — testing careful rule-following over memorized algorithm behavior. Don't assume standard FIFO/LRU/OPT mechanics without re-reading the question's exact rule.
- **Address translation via a small given page table** — decimal or binary logical addresses, direct lookup, "map user's view of memory" as a common second part.
- **fork() + demand paging interaction** (Fa25, both sets) — "max/min number of new pages created after fork() + a loop that writes to an array" — tests understanding that post-fork pages are shared (copy-on-write) until written, so minimum new pages can be 0 and maximum is bounded by the array's page count. This is a genuinely distinct question type from the standard replacement/translation drills — not yet in the module note in this depth, worth a mental note even without a full page fault/COW section.
- Shared pages (reentrant library code), internal fragmentation, and valid-invalid-bit reasoning appear as smaller (1–2 mark) conceptual items.

**Protection (Topics 29–30, when present)** — always small, always a single applied scenario: access-matrix right application (Owner/Copy, "draw the updated matrix after this right is used") or the "domain is static, how do you add rights after creation" scenario. Never a deep-recall question on revocation schemes, POSIX capabilities, or MAC/DAC taxonomy — those stay useful for completeness but are low-yield for marks relative to File Systems/Memory Management drilling.

**Security (Topics 31–32, when present — only Fa25 of the 4 recent sittings)** — buffer-overflow code-spotting ("which line is the vulnerability, justify why") and violation-*method* identification from a short scenario (matches the Security Violation Methods list added to Module 8 in this session's patch: masquerading, replay, MITM, session hijacking, privilege escalation). Cryptography, firewalls, and IDS never appear — consistent with the slide deck itself.

---

## Set A vs. Set B Pattern

Same as the Midterm's finding: **every sitting with two sets is structurally parallel** — identical question slots and mark allocations, with numbers and scenario framing swapped (e.g. Sp26: process-A-context-switch vs. process-B-context-switch with a different MCQ option list; Fa25/Sp25: different inode pointer-counts and byte sizes for the same max-file-size derivation). Practicing one set is direct practice for the other set's identical skill.

---

## Probable Questions for the Final — Ranked

<!-- Ranking rationale: frequency across the 8 sampled sittings, weighted almost entirely toward the current era (Sp25–Sp26, 4/4 sittings), since the pre-2025 pattern reads as a retired exam design. -->

**Near-certain — appeared in every current-era sitting sampled:**

1. **TLB Effective Access Time calculation** — solve for EAT, or solve for any one input (hit ratio, TLB time, memory time) given the rest. The single most reliable fixture across *both* eras of this exam.
2. **Inode byte/sector address calculation** — given inode size, block size, sector size, and reserved leading blocks, find an inode's disk address. Appeared in every File Systems section sampled in the current era.
3. **Multi-level paging derivation** — 2- or 3-level, deriving outer/inner/offset bit-widths from address bits + page size + entry size, then translating specific logical addresses through it.

**High probability — standard in the current era (3+ of last 4 sittings):**

4. **Inode max-file-size calculation**, possibly inverted (solve for an unknown pointer count given the max size).
5. **Crash-consistency deduction** from a partial-write scenario — which writes succeeded, what's the visible symptom.
6. **File access path timeline** — full I/O trace for a read or write scenario.
7. **Page replacement trace** (FIFO/LRU/Optimal) — read the question's rules carefully; a "buggy"/modified variant of the algorithm is common in the current era, not always the textbook-standard version.

**Moderate probability:**

8. **Access-matrix right application** (Protection) — Owner/Copy right used to update the matrix, or a static-domain scenario. Present in about half the current-era sittings.
9. **Page-table-size calculation** (single-level) — total page table size given address bits, page size, RAM size, entry overhead.
10. **Small pre-midterm identification sub-questions** folded into the Process/Sync-labeled early questions — a PCB field, a synchronization bug, a short fork()/output-trace item. Expect these light, not essay-depth.
11. **fork() + demand paging interaction** — max/min new pages after fork() and an array-writing loop (copy-on-write reasoning).

**Lower probability but still syllabus-legal:**

12. **Security questions** (buffer-overflow spotting, violation-method identification) — appeared in only 1 of 4 recent sittings, but syllabus-confirmed and cheap to prepare (small, single-scenario questions, not deep recall).
13. **Contiguous allocation (first-fit/best-fit/worst-fit)** — dormant since before Sp25 (0/4 recent), but was a fixture in the pre-2025 era and is still nominally in-scope per the topic map (see [CSE321_m6_memory_management](../concepts/CSE321_m6_memory_management.md) Topic 25's slide-absence note). Know the mechanics well enough not to lose an easy mark if it resurfaces.
14. **Banker's Algorithm / Resource Allocation Graph** — was heavily tested pre-2025 (Sp22 alone had ~15 marks of RAG+Banker's content) but has **zero appearances in the current era** and isn't in the current topic map at all. Lowest-probability item on this list; do not prioritize revision time here.
15. **Cryptography, firewalls, IDS/IPS** — never appeared in any sitting sampled, old or new, and the slide deck itself omits cryptography. Lowest-yield material in the entire post-Midterm syllabus.

---

## Revision Priority Implied by This Analysis

Items 1–7 map directly onto the Key Skills already built out in [CSE321_m5_file_systems](../concepts/CSE321_m5_file_systems.md) and [CSE321_m6_memory_management](../concepts/CSE321_m6_memory_management.md) during this session's slide/practice-sheet audit — those two notes are now the highest-leverage study material for this exam. Items 8, 10, and 12 are covered but genuinely lower-yield; don't let Protection/Security's thorough taxonomy coverage in [CSE321_m7_protection](../concepts/CSE321_m7_protection.md)/[CSE321_m8_security](../concepts/CSE321_m8_security.md) crowd out time that should go to File Systems/Memory Management drilling, since the actual mark distribution favors the latter two by a wide margin in every recent sitting.
