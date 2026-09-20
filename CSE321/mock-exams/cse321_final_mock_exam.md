---
course: CSE321
type: model-question-set
status: in-progress
assessment: Final
scope-confirmed: 2026-09-14
---

# CSE321 — Final Exam: Model Question Set (Full Syllabus)

## Scope

**Confirmed in-scope:** the full confirmed Final boundary, Topics 20–32 (`cse321_exam_notes.md`, official announcement 2026-09-06): File Systems (20–23) → Memory Management (24–28) → Protection (29–30) → Security (31–32). File Systems and Memory Management carry conceptual **and** math/simulation questions; Protection and Security are conceptual-only, per the same announcement.

**Confirmed exclusions:** none — this set targets the whole boundary, in the same order the syllabus (and the real exam, per every sampled sitting) presents it.

**Style anchor:** `CSE321_final_probable_questions.md`'s pattern analysis of 8 sampled Final sittings (Sp22–Sp26), weighted toward the current era (Sp25–Sp26, 4/4 sittings). Valid anchor — same creator pool (Central/BRACU) as this Final. The actual PYQ files are in `CSE321/pyq/` — variants below are built from the note's verified *structural* description of real PYQ questions (exact scenario wording, framing, mark split), not transcribed verbatim, and each is independently solved and checked against the relevant module note (`CSE321_m5`–`m8`) before being posed.

## Structure

| Slot | Section | Variants planned |
|---|---|---|
| 1 | File Systems (Topics 20–23) | (a) Inode byte/sector address calc, (b) Inverted inode max-file-size calc, (c) Crash-consistency deduction, (d) File access-path I/O trace |
| 2 | Memory Management (Topics 24–28) | (a) TLB Effective Access Time, (b) Multi-level paging derivation, (c) Page-replacement trace (hit/fault ratio), (d) Single-level page-table-size calc |
| 3 | Protection (Topics 29–30) | (a) Access-matrix copy-right propagation, (b) Static-domain "add rights after creation" scenario |
| 4 | Security (Topics 31–32) | (a) Buffer-overflow code-spotting, (b) Security-violation-method identification |

Slot order and relative weight mirror the real Final's own structure — File Systems and Memory Management are the two largest sections in every sampled current-era sitting (~30–45% each); Protection and Security are small, single-scenario sections (0–15% each).

## Grounding rule

Every variant must trace to a verified source before it is posed to the user — never invented and shown live:
- **A real PYQ problem**, if available — reuse its structure and style directly.
- **A verified worked example already sitting in the course's own concept note** — reuse via order-preserving relabeling (same underlying logic, new surface numbers/names) when a fresh derivation isn't needed for pedagogical variety, or a fresh derivation, fully solved and checked, when no reusable structure exists for that slot.

This exists because a wrong worked example actively misleads — a model question set gets trusted at face value during revision.

## Delivery

**Drafting:** every slot and variant is drafted in full below, upfront — no "(to be drafted)" stubs — so the whole paper's shape is visible before solving starts. Each variant carries a **Difficulty:** (Easy/Medium/Hard) tag, judged against this course's own PYQ pattern history in `CSE321_final_probable_questions.md`.

**Solving pace:** present one slot's cluster at a time in conversation, not the whole set at once. After the user attempts a variant, check it and record the verified answer directly in this file before moving to the next slot.

## Questions

### Slot 1 — File Systems (Topics 20–23)

#### (a) Inode Byte/Sector Address Calculation
*Grounded in `CSE321_m5_file_systems.md` Key Skill (2) — near-certain per PYQ ranking (#2, appeared in every File Systems section sampled in the current era).*

A disk uses block size = 4 KB, sector size = 512 B, and inode size = 128 B. The first 6 blocks of block group 0 are reserved (superblock, group descriptor table, block bitmap, inode bitmap — 6 blocks total) before the inode table begins.

**(a-i)** Find the byte address of inode number 60. [2 marks]
**(a-ii)** Find the sector address of inode number 60. [2 marks]

**Difficulty:** Medium — near-certain fixture, but requires the two-step byte→sector conversion, not just one formula.
**Status:** posed 2026-09-14, awaiting attempt.
**Answer:** _(pending)_

#### (b) Inverted Inode Max-File-Size Calculation
*Grounded in `CSE321_m5_file_systems.md` Key Skill (1) — high probability per PYQ ranking (#4), specifically flagged as often appearing in its inverted ("solve for the unknown pointer count") form.*

An inode has 4 direct pointers, 1 single indirect pointer, 1 double indirect pointer, and an unknown number of triple indirect pointers. Block size = 64 B, pointer size = 4 B. The inode's maximum file size is stated as **279,808 bytes**.

**(b)** How many triple indirect pointers does this inode have? Show your work. [3 marks]

**Difficulty:** Hard — the inverted direction (solve for pointer count given max size) explicitly flagged as the harder variant in `CSE321_final_probable_questions.md` (#4).
**Status:** posed 2026-09-14, awaiting attempt.
**Answer:** _(pending)_

#### (c) Crash-Consistency Deduction
*Grounded in `CSE321_m5_file_systems.md` Topic 23 (metadata-only/ordered journaling, the journal commit write-order rule) — PYQ rank #5.*

A file system uses **metadata-only (ordered) journaling**: for any update, the actual data block is written directly to its final on-disk location *before* the corresponding metadata (inode + bitmap changes) is journaled as a transaction (`TxB` → metadata blocks → `TxE`), which is only later checkpointed to the metadata's final location.

A process appends one new data block `D` to file `F`. The following happens, in order, before a sudden power loss:
1. Block `D` is written to its final location on disk, containing the new data.
2. `TxB` and the updated metadata blocks (new inode pointer + size, updated data bitmap) are written to the journal.
3. **Power loss occurs here** — before `TxE` is written to the journal.

**(c-i)** Was this transaction's metadata journaled successfully? Justify your answer using the specific rule that governs how recovery makes this decision. [2 marks]
**(c-ii)** After reboot and recovery, is block `D` a member of file `F` according to its inode? Describe the resulting symptom for block `D` (is its data present? is it marked used or free? is it reachable from `F`?). [3 marks]
**(c-iii)** Explain why ordered journaling deliberately writes the data block *before* journaling the metadata, rather than the reverse. [2 marks]

**Difficulty:** Hard — multi-step deduction chaining the journaling protocol to a concrete crash symptom, not a single-formula lookup.
**Status:** drafted, not yet posed (Slot 1(a)/(b) come first).
**Answer:** _(pending)_
- (c-i) No. Recovery uses the presence of `TxE` (not `TxB` or the metadata blocks themselves) to decide whether a transaction was fully logged — since `TxE` never reached the journal, the transaction is treated as incomplete and discarded on recovery.
- (c-ii) No — `F`'s inode was never updated (the metadata transaction was discarded), so it has no pointer to `D` and its size wasn't changed; `D` is not reachable from `F`. However, `D` physically contains the new data on disk (it was written directly, outside the journal), and the data bitmap was never updated either (that update was part of the discarded metadata transaction) — so `D` is likely still marked *free* in the bitmap despite holding real data: an orphaned block, invisible to the file system's own bookkeeping, at risk of being silently overwritten by a future allocation.
- (c-iii) If metadata were journaled and checkpointed *before* the data reached disk, a crash in between could leave `F`'s inode pointing to a block that doesn't yet contain the intended data (garbage or leftover content from whatever previously occupied that block) — a *consistent-looking but wrong* file. Writing data first guarantees that by the time any metadata commits to referencing it, the data genuinely is there.

#### (d) File Access-Path I/O Trace
*Grounded in `CSE321_m5_file_systems.md`'s verified `open()`/`write()`/`close()` I/O trace (Topic 21) — PYQ rank #6.*

A process executes `open("/home/logs/output.txt", O_WRONLY)` on an existing file, immediately followed by one `write()` call that appends a single new data block, then `close()`. Assume nothing is cached (no inode, directory entry, or data block already resident in memory) and the root inode's location is fixed/well-known (root inode number = 2).

**(d-i)** Enumerate the full step-by-step disk-access timeline for `open()`, stating what each read actually retrieves. [4 marks]
**(d-ii)** How many disk I/Os does the `write()` call itself require, and what are they? [2 marks]
**(d-iii)** What is the total number of disk I/Os across `open()` + `write()` + `close()`? [1 mark]

**Difficulty:** Medium — mechanical once the path-traversal pattern is learned, but requires correctly generalizing it to a 3-component path instead of reciting a memorized 2-component example.
**Status:** drafted, not yet posed (Slot 1(a)/(b) come first).
**Answer:** _(pending)_
- (d-i) 7 disk reads: (1) read block containing root inode, (2) read root's data block → find entry "home", (3) read block containing home's inode, (4) read home's data block → find entry "logs", (5) read block containing logs's inode, (6) read logs's data block → find entry "output.txt", (7) read block containing output.txt's inode. (Permission check and file-descriptor allocation follow, but involve no disk I/O.)
- (d-ii) 5 I/Os: read data bitmap → write data bitmap → read inode → write inode → write the new data block.
- (d-iii) 7 + 5 + 0 (`close()` is in-memory bookkeeping only) = **12 disk I/Os total.**

---

### Slot 2 — Memory Management (Topics 24–28)

#### (a) TLB Effective Access Time — Forward and Inverted
*Grounded in `CSE321_m6_memory_management.md` Topic 26 (EAT formula) — PYQ rank #1, the single most reliable fixture across every sampled sitting, both eras. The real pattern asks for either direction, not just forward EAT.*

A system's TLB has access time 10 ns; main memory access time is 90 ns.

**(a-i)** If the TLB hit ratio is 75%, compute the Effective Access Time. [2 marks]
**(a-ii)** Using the same TLB and memory access times, if the Effective Access Time is instead measured at 136 ns, find the TLB hit ratio. [3 marks]

**Difficulty:** Medium — the near-certain core-skill fixture; the inversion in (a-ii) is straightforward once the formula is rearranged rather than just plugged into.
**Status:** drafted, not yet posed (Slot 1 comes first).
**Answer:** _(pending)_
- (a-i) EAT = h(t+m) + (1−h)(t+2m) = 0.75×(10+90) + 0.25×(10+90+90) = 0.75×100 + 0.25×190 = 75 + 47.5 = **122.5 ns**.
- (a-ii) EAT = (t+2m) − h·m → 136 = 190 − 90h → 90h = 54 → **h = 0.6 (60%)**. Check: 0.6×100 + 0.4×190 = 60 + 76 = 136 ✓.

#### (b) Multi-Level Paging Derivation
*Grounded in `CSE321_m6_memory_management.md` Key Skill (3), Topic 26 — PYQ rank #3 (near-certain, the module's densest skill). Fresh numbers — not the practice sheet's own 8-bit/16 B/4 B/512 B example.*

A system has a 12-bit logical address space, page size 32 bytes, and page-table entry size 4 bytes (frame size = page size).

**(b-i)** How many levels of paging are needed so that every level's page table fits in exactly one frame? Derive the bit-width of each field (offset, and each page-number level). [4 marks]
**(b-ii)** Logical address `101101011010` (binary) is referenced. Split it into its fields and give each field's decimal value. [3 marks]

**Difficulty:** Hard — the densest single skill in this module; requires deriving the level count and per-level bit-widths from scratch, not recalling a fixed example.
**Status:** drafted, not yet posed (Slot 1 comes first).
**Answer:** _(pending)_
- (b-i) Offset bits n = log2(32) = 5. Page-number bits P = 12 − 5 = 7. Bits per level k = log2(frame size / entry size) = log2(32/4) = log2(8) = 3. Levels = ceil(7/3) = **3 levels**. Splitting P into 3-bit chunks from the offset outward: innermost (p3) = 3 bits, middle (p2) = 3 bits, outermost (p1) takes the leftover = 7−3−3 = 1 bit. Field layout: `p1 (1 bit) | p2 (3 bits) | p3 (3 bits) | d (5 bits)` = 12 bits total ✓. Check: each of p2's/p3's tables has 2³=8 entries × 4 B = 32 B = exactly one frame ✓; p1's table has 2¹=2 entries × 4 B = 8 B, which fits within a frame without needing to fill it.
- (b-ii) `101101011010` splits as `1 | 011 | 010 | 11010` → p1 = 1 (binary "1"), p2 = 3 (binary "011"), p3 = 2 (binary "010"), d = 26 (binary "11010"). Translation: outer table index 1 → pointer to a middle table; middle table index 3 → pointer to an inner table; inner table index 2 → frame number F; physical address = F×32 + 26.

#### (c) Page-Replacement Trace — Modified Rule
*Grounded in `CSE321_m6_memory_management.md` Topic 28's trace method — PYQ rank #7, explicitly flagged as often appearing with a deliberately non-standard replacement rule to test careful rule-following over memorized algorithm behavior.*

A system has 3 frames, initially empty, and uses a non-standard replacement rule: **Most Recently Used (MRU)** — on a page fault with all frames full, evict the page that was referenced most recently among the currently resident pages (the opposite of LRU). Reference string: `1 2 3 4 1 2 5 1 2 3 4 5`.

**(c-i)** Trace the resident-page set after each reference, marking each as a hit or fault. [5 marks]
**(c-ii)** Report the resulting fault ratio and hit ratio as percentages. [2 marks]

**Difficulty:** Hard — deliberately designed to trap the default assumption of standard LRU/FIFO behavior; requires tracking "most recently used" precisely at each step.
**Status:** drafted, not yet posed (Slot 1 comes first).
**Answer:** _(pending)_

| Ref | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Resident (after) | {1} | {1,2} | {1,2,3} | {1,2,4} | {1,2,4} | {1,2,4} | {1,4,5} | {1,4,5} | {4,5,2} | {4,5,3} | {4,5,3} | {4,5,3} |
| Fault? | ● | ● | ● | ● | | | ● | | ● | ● | | |

(At ref 4: frames full {1,2,3}, MRU=3 → evict 3, load 4. At ref 5: frames full {1,2,4}, MRU=2 → evict 2, load 5. At ref 2 (9th ref): frames full {1,4,5}, MRU=1 → evict 1, load 2. At ref 3 (10th ref): frames full {4,5,2}, MRU=2 → evict 2, load 3.)

- (c-i) 7 faults (refs 1,2,3,4,5,2,3 in that positional order), 5 hits (refs 1,2,1,4,5 in that positional order).
- (c-ii) Fault ratio = 7/12 ≈ **58.33%**. Hit ratio = 5/12 ≈ **41.67%**.

#### (d) Single-Level Page-Table Size
*Grounded in `CSE321_m6_memory_management.md` Topic 26 — PYQ rank #9 (moderate probability).*

A process has a 24-bit logical address space and a page size of 4 KB. Physical memory (RAM) is 64 MB, and each page-table entry needs just enough bits to hold a frame number, plus 2 extra bits (valid + protection).

**(d)** Compute the total size, in bytes, of this process's (single-level) page table, assuming it uses its full logical address space. [4 marks]

**Difficulty:** Medium — a single, moderate-probability combination of frame-count and page-count reasoning, less dense than multi-level paging.
**Status:** drafted, not yet posed (Slot 1 comes first).
**Answer:** _(pending)_
- Offset bits n = log2(4096) = 12. Page-number bits = 24 − 12 = 12 → **4,096 page-table entries** (one per possible page number). Number of physical frames = RAM / page size = 64 MB / 4 KB = 2²⁶/2¹² = 2¹⁴ = 16,384 → needs **14 bits** to number a frame. Entry size = 14 + 2 = 16 bits = 2 bytes. Total page-table size = 4,096 × 2 B = **8,192 bytes (8 KB)**.

---

### Slot 3 — Protection (Topics 29–30)

#### (a) Access-Matrix Copy-Right Propagation

A system has domains D1, D2, D3 and objects `File_X`, `File_Y`. The current access matrix is:

| | File_X | File_Y |
|---|---|---|
| **D1** | {read\*, write} | {owner} |
| **D2** | {} | {read} |
| **D3** | {} | {} |

Two operations happen, in this order:
1. D1 uses its copy right to give D3 the `read` right on `File_X` — **including** the asterisk.
2. D3 then uses that same right to give D2 the `read` right on `File_X` — **without** the asterisk.

**(a-i)** Draw the access matrix after both operations. [3 marks]
**(a-ii)** Can D2 now further copy the `read` right on `File_X` to another domain? Justify in one line. [2 marks]

**Difficulty:** Medium — a moderate-probability item (appears in about half of recent sittings); requires tracking the asterisk's propagation across two chained copy operations, not a single lookup.
**Status:** drafted, not yet posed (Slot 1 comes first).
**Answer:** _(pending)_

#### (b) Static-Domain Rights Addition

D1 and D2 are **static** domains (their object associations are fixed once created). D1 currently holds only `{read}` on `File_Z`. The process running in D1 now also needs `write` on `File_Z` to finish a new task — but D1's own definition cannot simply be edited or reconstructed on the fly.

**(b-i)** Name two special access rights (from this module) that could still let D1's entry for `File_Z` gain `write`, without D1 itself needing to become a dynamic domain. [2 marks]
**(b-ii)** Briefly explain how one of them accomplishes this. [2 marks]

**Difficulty:** Medium — conceptual, but requires recalling and correctly naming two specific special-rights mechanisms rather than a single definition.
**Status:** drafted, not yet posed (Slot 1 comes first).
**Answer:** _(pending)_

---

### Slot 4 — Security (Topics 31–32)

#### (a) Buffer-Overflow Code-Spotting
*Grounded in `CSE321_m8_security.md` Topic 31, Program Threats (Stack/Buffer Overflow) — PYQ rank #12 (low-medium probability, appeared in only 1 of 4 recent sittings, but small and cheap to prepare).*

```c
void store_username(char *input) {
    char username[16];
    strcpy(username, input);   // <-- Line X
    printf("Welcome, %s\n", username);
}

int main() {
    char buffer[256];
    fgets(buffer, sizeof(buffer), stdin);
    store_username(buffer);
    return 0;
}
```

**(a)** Identify the line containing a buffer-overflow vulnerability. Explain (i) why it is vulnerable, and (ii) what an attacker could achieve by exploiting it, naming the specific mechanism from this module's Program Threats material. [3 marks]

**Difficulty:** Easy — single-line identification plus a definition-level justification, matching this module's small, single-scenario PYQ format.
**Status:** drafted, not yet posed (Slot 1 comes first).
**Answer:** _(pending)_
- The vulnerable line is `strcpy(username, input)`. `username` is a fixed 16-byte stack buffer, but `input` (sourced from up to 256 bytes read via `fgets` in `main`) can be far longer, and `strcpy` performs no bounds checking — writing past `username`'s end overflows into adjacent stack memory, potentially overwriting the function's return address. An attacker who controls `input`'s content could use a **trampoline** technique to redirect execution into attacker-supplied injected code, exactly the mechanism named under Stack/Buffer Overflow in this module.

#### (b) Security Violation-Method Identification
*Grounded in `CSE321_m8_security.md` Topic 31, Security Violation Methods — PYQ rank #12.*

An attacker intercepts network traffic between a bank's server and an already-logged-in customer. Without ever learning the customer's password, the attacker begins issuing transaction requests using the customer's already-authenticated session token, which was captured mid-transit.

**(b)** Identify which security violation *method* (not category) this is, and state in one line what specifically distinguishes it from a Man-in-the-Middle attack. [2 marks]

**Difficulty:** Easy — direct identification from a short, named list once the scenario is read carefully.
**Status:** drafted, not yet posed (Slot 1 comes first).
**Answer:** _(pending)_
- **Session hijacking** — the attacker takes over an already-authenticated session using the captured token, rather than sitting inside the ongoing communication flow and impersonating both ends simultaneously (which is Man-in-the-Middle). Session hijacking specifically bypasses the need to authenticate at all, by riding on a session that has already passed that step.

---

## After completion

Fold the finished set + verified answers into the CSE321 revision guide (if one exists, per `project_revision_guide_workflow`) as a "Mock Final" section.
