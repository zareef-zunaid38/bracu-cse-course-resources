---
type: concept
course: CSE370
topic: "Concurrency Control (Problems, Locking, Deadlock, MVCC)"
difficulty: 3
thesis_relevance: —
status: draft
date: 2026-09-06
prerequisites: ["CSE370_t23_transactions.md"]
---

# Concurrency Control
**Course:** CSE370 — Database Systems
**Date:** 2026-09-06

**Scope note:** Covers Topic 24 (Concurrency Control) from `cse370_topics.md`. The material spans two lecture decks: concurrency problems, SQL isolation levels, and Multiversion Concurrency Control (MVCC) are introduced at the end of `CSE370_Lecture_09_transactions.pdf` (see `CSE370_t23_transactions.md` for that deck's Topic 23 content); lock-based protocols (2PL), deadlock handling, and graph-based protocols are the entirety of `CSE370_Lecture_10_concurrency_control.pdf`. Confirmed not on Midterm; confirmed Quiz 4 syllabus (2026-09-07, alongside Transactions). Final syllabus follows by elimination from the confirmed Midterm boundary (Relational Algebra, Lecture 7) — everything from Lecture 9 onward is Final-track.

**Sources used:** `CSE370_Lecture_09_transactions.pdf` and `CSE370_Lecture_10_concurrency_control.pdf` (both read via their sidecars, per the vault's sidecar rule), `CSE370_practice_ps9_transactions.pdf` (17 questions with solutions — mixes Transactions and Concurrency Control scenarios; **updated 2026-09-13**, Q1–17 cited below unchanged, new Q18–19 added for Timestamp-Ordering practice), and **new 2026-09-13**: `CSE370_practice_ps10_concurrency_control.pdf` (10 questions — 2PL growing/shrinking/lock-point/upgrade-downgrade graph-reading, deadlock conditions, Wait-Die/Wound-Wait). No PYQs exist for CSE370 (confirmed).

> [!confirmed] Final syllabus officially confirmed 2026-09-13: Lecture 10 in full (all slides). **MV2PL (below) is confirmed OUT of scope** — see the flag at that section; only the Timestamp-Ordering MVCC technique (documented in `CSE370_t23_transactions.md`, pages 26–28 of Lecture 9) remains examinable.

> [!note] Textbook sourcing corrected 2026-09-06 (audit): `cse370_resources.md` maps this topic to Silberschatz 6e Ch 15, but the HDD copy of that PDF is a 94-page front-matter excerpt that ends at Chapter 3 — Ch 15 is not actually present. That citation has been removed rather than left implying the chapter was read. This note is built entirely from the lecture slides and practice sheet above.

---

## Definition

**Concurrency control** is the set of mechanisms a DBMS uses to control the interaction among transactions executing at the same time, preventing them from destroying the consistency of the database. Multiple transactions running concurrently improve processor/disk utilization and reduce average response time (short transactions don't wait behind long ones) — but without control, several distinct correctness problems can occur.

---

## Key Properties

### The Five Concurrency Control Problems

| Problem | What happens | Root cause |
|---|---|---|
| **Lost Update** | Two transactions read the same item and both write updates; one overwrites (loses) the other's update | Both transactions read *before* either writes |
| **Temporary Update / Dirty Read** | A transaction reads a value written by another, *uncommitted* transaction; that writer later rolls back | Reading uncommitted data |
| **Incorrect Summary** | An aggregate reads some items before another transaction updates them and others after | Aggregate spans a concurrent partial update |
| **Non-Repeatable Read** | The same transaction reads the same row twice and gets two different values | Another transaction committed an update to that row in between |
| **Phantom Read** | The same transaction re-runs a filtering query and gets a different *set of rows* | Another transaction inserted/deleted a matching row in between |

**Lost Update example:** T1 reads X, computes `X+15`; T2 reads the same (pre-T1-write) X, computes `X-25` and writes it; T1 then writes its own computed value, overwriting T2's update — T2's subtraction is lost.

**Temporary Update example:** T1 updates a balance (uncommitted), T2 reads that uncommitted value, then T1 rolls back — T2 acted on data that never existed in the committed database.

**Incorrect Summary example:** T3 is summing X and Y while T1 is transferring an amount from X to Y; if T3 reads X *after* T1's subtraction but Y *before* T1's addition, the sum comes out short by that amount.

**Non-Repeatable Read vs. Phantom Read — the key distinction:** Non-Repeatable Read is about a **value** changing on re-read of the *same row*; Phantom Read is about the **row set** changing (a new row appearing or an old one vanishing) on re-running the *same query*. Both require the first transaction to read twice, but one is a single-row problem and the other is a result-set problem.

### SQL Isolation Levels

Isolation levels trade correctness guarantees for concurrency/performance. Set at the database level, or changeable at transaction start.

| Isolation Level | Dirty Read | Non-Repeatable Read | Phantom Read |
|---|---|---|---|
| **Serializable** (strictest) | Prevents | Prevents | Prevents |
| **Repeatable Read** | Prevents | Prevents | **Allows** |
| **Read Committed** (default in many systems) | Prevents | Allows | Allows |
| **Read Uncommitted** (loosest) | Allows | Allows | Allows |

- **Serializable:** transactions behave as if executed one at a time, even though physically concurrent. Prevents all three read phenomena. Strictest, worst performance; default in some systems.
- **Repeatable Read:** only committed records readable; repeated reads of the *same record* return the same value — but a completely *new* row inserted by another transaction can still appear (phantom).
- **Read Committed:** only committed records readable, but successive reads of the same record can return *different* committed values if another transaction updated and committed in between.
- **Read Uncommitted:** even uncommitted records can be read — fastest, but allows all three problems.

**Critical caveat (easy to miss):** Lost Update and Incorrect Summary aren't in the three-phenomena table above at all — that table only tracks dirty read / non-repeatable read / phantom read. **Serializable prevents all concurrency problems**, Lost Update and Incorrect Summary included, since it forces transactions to behave as if fully serialized. But isolation levels **below** Serializable do **not**, by themselves, guarantee prevention of Lost Update or Incorrect Summary — at those lower levels, preventing them needs additional techniques (explicit locking, atomic update statements, careful query/coding style) layered on top.

### Lock-Based Concurrency Control

A **lock** controls access to a data item; a transaction must acquire a lock before reading/writing (ask permission before use). Only *compatible* locks may be held simultaneously by different transactions.

| | S (Shared) | X (Exclusive) |
|---|---|---|
| **S** | Compatible ✓ | Incompatible ✗ |
| **X** | Incompatible ✗ | Incompatible ✗ |

- **Shared lock (S-lock):** read-only; multiple transactions can hold it on the same item at once.
- **Exclusive lock (X-lock):** read/write; blocks every other transaction (S or X) from that item.

### Two-Phase Locking (2PL)

| Phase | Rule |
|---|---|
| **Growing** | Transaction acquires all locks it needs; cannot release any lock |
| **Shrinking** | Begins at the first lock release; can only release locks, never acquire/upgrade new ones |

- **Lock point:** the exact moment a transaction gets its *final* lock — the boundary between growing and shrinking.
- **Upgrading** (S→X): only during the growing phase, and only if no other transaction holds an S-lock on that item.
- **Downgrading** (X→S): only during the shrinking phase (reduces restriction, generally allowed).

**Categories of 2PL:**

| Variant | Rule | Effect |
|---|---|---|
| **Strict 2PL** | All X-locks held until commit/abort; S-locks may release earlier | Prevents dirty reads, no cascading rollback |
| **Rigorous 2PL** | *All* locks (S+X) held until commit | Stronger than strict; locks released only after finishing |
| **Conservative 2PL** | All required locks acquired upfront before starting; if unavailable, wait (no partial locking) | Deadlock-free, guarantees serializability, but low concurrency/practicality |

### Deadlock

A **deadlock** is a state where transactions block each other permanently because each holds a resource the other needs (T1 holds A, needs B; T2 holds B, needs A → circular wait). Consequences: transactions stay blocked forever, throughput drops, resources sit locked unused, and in severe cases the whole system stalls.

**Necessary conditions** (all three must hold for deadlock to occur):
1. **Hold and Wait** — a transaction holding one resource requests another held elsewhere.
2. **No Preemption** — a resource can't be forcibly taken; only voluntarily released.
3. **Circular Wait** — a cycle exists where each transaction waits on the next.

**Handling strategies:**

| Strategy | Idea |
|---|---|
| **Avoidance** | Design so deadlock structurally can't occur: (1) always lock resources in a fixed order (e.g. always A before B); (2) use finer-grained locks (row, not table) and appropriate isolation levels — reduces but doesn't eliminate risk |
| **Detection** | Allow deadlocks to happen but detect them — build a **Wait-For Graph** (transactions = nodes, an edge = "waiting for"); a cycle confirms deadlock |
| **Prevention** | Structurally disallow the conditions using transaction timestamps — see Wait-Die/Wound-Wait below |
| **Recovery** | Once detected, pick a victim (lowest rollback cost), roll it back (total or partial rollback) to break the cycle |
| **Timeout-based** | A transaction waits only a fixed time; if exceeded, roll it back — simple, but may roll back non-deadlocked transactions and can still allow starvation |

**Deadlock prevention schemes** (T1 = older, T2 = younger, by timestamp):

| Scheme | Preemptive? | Rule |
|---|---|---|
| **Wait-Die** | No | An *older* transaction may wait for a younger one's resource; a *younger* one requesting an older one's resource is aborted and restarted |
| **Wound-Wait** | Yes | An *older* transaction can force-abort ("wound") a younger one holding what it needs; a *younger* one requesting an older one's resource must wait |

Both avoid cycles by using transaction age as a strict ordering — a younger transaction is never allowed to make an older one wait *and* block on it simultaneously. **Aborted/restarted transactions keep their original timestamp** on restart, to reduce starvation risk.

**Starvation problem:** the same transaction keeps getting picked as the deadlock victim. **Solution:** track each transaction's rollback count, raise its priority after each rollback, and factor in both age and rollback count when choosing the next victim.

### Multiversion Concurrency Control (MVCC)

The DBMS keeps multiple physical versions of each data item so readers and writers don't block each other: a read picks whichever version preserves serializability; a write creates a new version, retaining the old one(s). Drawback: extra storage — mitigated with temporary storage / vacuum-style cleanup.

1. **Timestamp-ordering technique:** each version Xi tracks `read_TS(Xi)` (largest timestamp of any transaction that read it) and `write_TS(Xi)` (timestamp of whoever wrote it).
   - `read_item(X)` by Ti: if `write_TS(X) > TS(Ti)` → **rollback Ti** (it would read a version from its own future); else `read_TS(X) = MAX(read_TS(X), TS(Ti))`.
   - `write_item(X)` by Ti: if `read_TS(X) > TS(Ti)` OR `write_TS(X) > TS(Ti)` → **rollback Ti** (a later transaction already read/wrote past this write); else `write_TS(X) = TS(Ti)`.
2. **Multiversion 2PL with Certify Locks (MV2PL) — CONFIRMED OUT OF SCOPE for the Final (2026-09-13):** the official central announcement caps Lecture 9 at "Pages 1–28"; MV2PL is on page 29 of the 30-page deck, verified by direct inspection. The Timestamp-Ordering technique above (pages 26–28) remains in scope. Retained below for completeness/Quiz 4 reference only — do not prioritize for Final review. Three lock modes — read, write, certify (compatibility: only Read-Read is compatible; everything touching Write or Certify is not). Two versions per item: **committed version X** (original) and **local version X′** (private copy made when a transaction takes a write lock). Other transactions keep reading committed X while the writer works on X′. To commit, the writer needs a **certify lock**, incompatible with read locks — so commit waits until all current readers finish. Once granted, X′ replaces X as the new committed version.

### Graph-Based Concurrency Control (alternative to 2PL)

Defines a **fixed access order** for data items instead of a growing/shrinking discipline: data items = nodes, allowed access order = directed edges. If A→B, any transaction touching both must access A before B. The graph must be **acyclic** (A→B→C→A is disallowed; A→B→C is fine).

- **Properties:** conflict-serializable (the fixed order guarantees it); deadlock-free (acyclic order prevents circular wait); may still cause cascading rollback (recoverability not guaranteed); limitation — access order must be fixed in advance.
- **Tree Protocol** (simplest graph-based protocol): X-locks only (no shared locks). First lock can be on any item; every subsequent lock must be on a **child** of an item already locked by that transaction (must hold the parent before locking a child). Unlocking allowed any time — no strict growing/shrinking phases. Once unlocked, an item can never be relocked by the same transaction.

---

## Worked Example

**2PL trace with dirty reads and rollbacks** (from Lecture 10's worked example, T1/T2/T3 on items A and B):
1. T1: `Lock-X(A)`, `Read(A)`, `Write(A)`.
2. T1: `Lock-S(B)` — this is T1's **lock point**. `Read(B)`. `Unlock(A), Unlock(B)`.
3. Meanwhile, T2 tries `Read(A)` while T1 still holds it uncommitted → **dirty read** → **T2 rolls back**.
4. After T1 releases A, T2 legitimately does `Lock-X(A)` (its lock point), `Read(A)`, `Write(A)`, `Unlock(A)`.
5. T3 tries `Read(A)` while T1 still held it → dirty read → **T3 rolls back**; T3 later legitimately does `Lock-S(A)` (its lock point) once both T1 and T2 have released A, then `Read(A)`.

Takeaway: any read of an item another transaction currently holds an uncommitted X-lock on is a dirty read and forces a rollback — the exam-relevant pattern is spotting *which* reads in a given trace happen before vs. after the writing transaction's unlock step.

**Concurrency-problem identification** (from `CSE370_practice_ps9_transactions.pdf`, a recurring exam question type): given two transactions' interleaved operations, name the problem and justify with the transaction states. E.g. Q10 (Alice deducts $50, Bob deposits $30, both based on reading the same $100 starting balance, one commit overwrites the other) → **Lost Update**, because both transactions read before either wrote, so one write is silently discarded. Q11 (Alice updates a salary, Bob reads it before Alice's transaction is validated and rolled back by a system exception) → **Temporary Update / Dirty Read**, because Bob read a value from a transaction that never committed.

**Combined multi-problem scenario** (`CSE370_practice_ps9_transactions.pdf`, Q14 — a more complex exam shape than the single-problem identifications above): T1 (Teller) reads Customer X's balance ($150), then T2 (Auditor) reads the same $150 while generating a report. T1 deducts $100 (balance now $50, **not yet committed**) — T2 re-reads and sees this uncommitted $50 → **Dirty Read**. T1 then rolls back (balance reverts to $150). Meanwhile T2 inserts a new Customer Y with balance $80, then re-runs its "balance below $100" query and now sees Customer Y, a row that wasn't in the first run → **Phantom Read**. Both problems trace back to the same violated property: **Isolation**. Prevention: run at **Serializable** isolation so the transactions execute as if sequential. Recovery-type: an **undo log** — it keeps the old value ($150), which is exactly what's needed to revert T1's uncommitted deduction.

**Wait-For Graph deadlock detection:** T1 holds Account A, wants Account B; T2 holds Account B, wants Account A. Graph: T1→T2 (needs B), T2→T1 (needs A) — this 2-node cycle confirms deadlock. The same reasoning generalizes to any N-transaction cycle: a wait-for graph edge exists between any two transactions where one blocks on a resource the other holds, and a cycle anywhere in the graph is sufficient to declare deadlock (no minimum cycle length beyond 2).

---

## Connections
- **Prerequisites:** Transactions & ACID Properties (Topic 23) — `CSE370_t23_transactions.md`. Isolation (the ACID property) is the correctness goal; everything in this note is a mechanism for achieving it.
- **Successors:** Query Processing and Optimization (Topic 22, despite the lower topic number — taught after in lecture order) — locking granularity interacts with query plan choices, though this connection isn't elaborated in the slides.
- **Research relevance:** — (core DBMS infrastructure; no direct connection to the thesis's AI-agents/knowledge-systems research direction).

---

## Common Mistakes

- Confusing Non-Repeatable Read with Phantom Read — Non-Repeatable Read is a **value** changing on the same row; Phantom Read is the **row set** changing (new/vanished rows) on the same query. Both need a second read/query by the same transaction, but the "what changed" is different.
- Assuming Repeatable Read isolation prevents phantoms — it doesn't; only Serializable prevents all three named phenomena (dirty read, non-repeatable read, phantom read).
- Assuming an isolation level *below* Serializable prevents Lost Update or Incorrect Summary — these two problems aren't covered by the standard three-phenomena table at all, so anything less than Serializable needs additional application-level or locking discipline on top. (Serializable itself does prevent them, since it prevents every concurrency problem — don't overcorrect into thinking no isolation level can.)
- Mixing up Wait-Die and Wound-Wait — Wait-Die is non-preemptive (younger transactions abort themselves rather than force anything); Wound-Wait is preemptive (older transactions can force-abort younger ones). Both prevent deadlock, but by opposite mechanisms.
- Forgetting that aborted/restarted transactions in deadlock prevention keep their **original timestamp** — assigning a new timestamp on restart would let a transaction be repeatedly "outrun" by newer arrivals, defeating the starvation-avoidance purpose of the scheme.
- In the Tree Protocol, assuming a transaction can lock any node once it holds *some* lock — it must specifically hold the lock on that node's **parent**, not just any ancestor or any lock at all (see the worked Lecture 10 example where T2 cannot lock D despite already holding C and G, because it never held D's parent, B).

---

## Anki Cards

START
Basic
What's the difference between the Lost Update problem and the Temporary Update (Dirty Read) problem?
Back: Lost Update — two transactions both read the same item before either writes, so one's write silently overwrites (loses) the other's. Temporary Update — a transaction reads a value written by another, still-uncommitted transaction, and that writer later rolls back, leaving the reader having used data that never really existed.
END

START
Basic
Which SQL isolation level prevents Dirty Reads and Non-Repeatable Reads but still allows Phantom Reads?
Back: Repeatable Read. Only Serializable (the strictest level) prevents all three phenomena — dirty read, non-repeatable read, and phantom read.
END

START
Basic
Do isolation levels below Serializable guarantee prevention of the Lost Update and Incorrect Summary problems?
Back: No. Those two problems aren't part of the standard three-phenomena isolation-level table (dirty read / non-repeatable read / phantom read) at all — preventing them needs additional locking or application-level techniques regardless of isolation level.
END

START
Basic
In Two-Phase Locking, what is the "lock point" and what changes after it?
Back: The lock point is the moment a transaction acquires its final lock — the boundary between the growing phase (acquiring locks only) and the shrinking phase (releasing locks only, no new acquisitions or upgrades allowed after this point).
END

START
Basic
What's the key difference between the Wait-Die and Wound-Wait deadlock prevention schemes?
Back: Wait-Die (non-preemptive): an older transaction may wait for a younger one's resource; a younger one requesting an older one's resource is aborted and restarted. Wound-Wait (preemptive): an older transaction can force-abort ("wound") a younger one holding what it needs; a younger one requesting an older one's resource must wait instead.
END

START
Basic
In the Tree Protocol (a graph-based concurrency control method), what rule governs which item a transaction can lock next?
Back: Only exclusive (X) locks are used. The first lock can be on any item, but every subsequent lock must be on a child of an item the transaction already holds a lock on — it must hold the parent before locking the child. Once an item is unlocked, that transaction can never relock it.
END

START
Basic
What are the three necessary conditions for a deadlock to occur?
Back: Hold and Wait (holding one resource while requesting another held elsewhere), No Preemption (a resource can't be forcibly taken, only voluntarily released), and Circular Wait (a cycle of transactions each waiting on the next).
END

START
Basic
A Teller transaction (T1) and an Auditor transaction (T2) both read a balance; T1 deducts money (uncommitted) and T2 re-reads that uncommitted value, then T1 rolls back; separately T2 inserts a new row and its re-run report query now includes it. Name both concurrency problems and the single ACID property they both violate.
Back: T2 reading T1's uncommitted deduction is a Dirty Read (Temporary Update); T2's report query returning a new row on re-run is a Phantom Read. Both trace back to a violation of Isolation.
END
