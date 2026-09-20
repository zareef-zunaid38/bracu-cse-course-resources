# CSE370 Lecture 9: Transactions

BRAC University CSE370: Database Systems slide deck, 30 slides. Clean digital slides
(dark theme, decorative shapes), fully legible — transcribed with confidence. Covers
Topic 23 (Transactions/ACID) fully, then bleeds into Topic 24 (Concurrency Control)
material — concurrency problems, SQL isolation levels, and Multiversion Concurrency
Control (MVCC) — before Lecture 10 picks up lock-based protocols. Confirmed Quiz 4
syllabus (2026-09-07) alongside Lecture 10.

## Content

**Transaction definition:** a logical unit of database processing containing one or
more read/write operations. Worked running example throughout: transferring $50 from
account A to B via `read(A), A:=A-50, write(A), read(B), B:=B+50, write(B)`.

**ACID properties:**
- **Atomicity:** all-or-nothing execution. Enforced by the DBMS recovery subsystem — a
  failed transaction must have all its partial effects undone.
- **Consistency:** a transaction takes the DB from one consistent state to another
  (explicit constraints like PK/FK, and implicit ones like "sum of A+B unchanged").
  The DB may be temporarily inconsistent mid-transaction; must be consistent after.
- **Isolation:** concurrent transactions must not see each other's uncommitted
  updates. Enforced by the concurrency control subsystem. Trivially achieved by
  serial execution, but concurrent execution is preferred for performance.
- **Durability:** once committed, changes must survive any subsequent hardware/software
  failure. Enforced by the recovery subsystem.

**Transaction Processing — two main issues:** Recovery (handling failures/crashes) and
Concurrency Control (handling simultaneous transactions).

**Why recovery is needed — causes of transaction failure:**
1. Computer failure / system crash (hardware or software error; internal memory
   contents may be lost).
2. Transaction or system error (integer overflow, division by zero, bad parameters,
   logical bugs, user-interrupted execution).
3. Local errors/exceptions detected by the transaction itself (data not found,
   insufficient balance → cancel; or a programmed abort).
4. Concurrency control enforcement (aborted to preserve serializability, or to break
   a deadlock).
5. Disk failure (read/write malfunction, head crash).
6. Physical problems/catastrophes (power/AC failure, fire, theft, sabotage, wrong
   tape mounted, overwritten media).

**Transaction states (recovery):** Active (executing reads/writes) → Partially
Committed (ended, pending final checks) → Committed (durable) — the success path. Or
Active → Failed (a check fails, or an abort) → Terminated — the failure path, possibly
requiring rollback to undo WRITE effects. Failed/aborted transactions may be restarted
later as brand-new transactions. Standard diagram: Begin→Active→(Read/Write loop)→End
transaction→Partially committed→Commit→Committed; Abort branches from Active or
Partially committed into Failed→Terminated.

**Log-based recovery:** a log is a sequence of log records on stable storage recording
update activity. Record types: `<Ti start>` (transaction registers itself),
`<Ti, X, V1, V2>` (before writing X, records old value V1 and new value V2), `<Ti
commit>` (written when Ti's last statement finishes). Example log/write trace shown for
T0 (writes A: 1000→950, B: 2000→2050, commits) and T1 (writes C: 700→600, commits).
- **undo/rollback(Ti):** restores all data items Ti updated to their old values,
  walking the log backwards from Ti's last record; each restoration writes a special
  `<Ti, X, V>` record; finishes with `<Ti, abort>`.
- **redo(Ti):** re-applies all of Ti's updates to their new values, walking the log
  forward from Ti's first record.

**Concurrency control problems/phenomena** (occur without concurrency control
mechanisms):
- **Lost Update:** two transactions read the same item and both write updated
  values; one write overwrites (loses) the other's update. Diagram: T1 and T2 both
  `read_item(X)`, both compute new X, T2's `write_item(X)` overwrites T1's earlier
  write of X.
- **Temporary Update / Dirty Read:** a transaction reads a value written by another,
  *uncommitted* transaction; if that writer later rolls back, the reader used data
  that never really existed in the DB. Example: T1 updates a balance, T2 reads it
  (dirty read), T1 rolls back — T2 saw a value that was never committed.
- **Incorrect Summary / Incorrect Analysis:** an aggregate transaction reads some
  items before another transaction updates them and other items after, producing a
  wrong summary (off by the amount transferred). Example: T3 sums X and Y while T1
  is mid-transfer between them.
- **Non-Repeatable Read:** a transaction reads the same row twice within itself and
  gets two different values because another transaction committed an update
  in between. Example: T1 selects balance=$100, T2 updates+commits to $200, T1
  re-selects and now gets $200 — not repeatable.
- **Phantom Read:** a transaction re-runs the same filtering query and gets a
  different *set of rows* (not just different values) because another transaction
  inserted/deleted rows matching the filter in between.

**Transactions in SQL:**
- A single SQL statement is always atomic (completes fully or leaves the DB
  unchanged).
- No explicit `BEGIN TRANSACTION` — a transaction starts implicitly on certain
  statements. Every transaction needs an explicit end: `COMMIT` (commits current,
  starts a new one) or `ROLLBACK` (aborts current).
- By default, most DBs auto-commit every successful statement; this can be turned
  off via a directive.
- **Isolation levels** control how much a transaction is isolated from others'
  writes; settable at the DB level or at transaction start.

**SQL isolation levels (strictest to loosest) and what each prevents:**

| Isolation Level | Dirty Read | Non-Repeatable Read | Phantom Read |
|---|---|---|---|
| Serializable | Prevents | Prevents | Prevents |
| Repeatable Read | Prevents | Prevents | **Allows** |
| Read Committed (default in many systems) | Prevents | Allows | Allows |
| Read Uncommitted (loosest) | Allows | Allows | Allows |

Serializable ensures transactions behave as if run one at a time (strictest,
worst performance). Repeatable Read guarantees the same row returns the same value on
repeat reads, but a *new matching row* can still appear (phantom). Read Committed only
guarantees committed data is read — repeat reads of the same row can differ. Read
Uncommitted permits reading uncommitted (dirty) data — fastest, weakest guarantees.
Note: isolation levels below Serializable do not by themselves prevent Lost Update or
Incorrect Summary — those need additional application-level or locking techniques.

**Multiversion Concurrency Control (MVCC):** the DBMS keeps multiple physical versions
of a data item so readers and writers don't block each other. A read picks the version
that preserves serializability; a write creates a new version, retaining old version(s).
Drawback: extra storage — mitigated via temporary storage / vacuum-style cleanup.

1. **Multiversion Technique Based on Timestamp Ordering:** each version Xi tracks
   `read_TS(Xi)` (largest timestamp of any transaction that successfully read it) and
   `write_TS(Xi)` (timestamp of the transaction that wrote it). FIFO transaction order
   assumed (e.g. T1→T2 by increasing timestamp).
   - On `read_item(X)` by Ti: if `write_TS(X) > TS(Ti)`, rollback Ti (it would be
     reading a version written "in its future"); else set
     `read_TS(X) = MAX(read_TS(X), TS(Ti))`.
   - On `write_item(X)` by Ti: if `read_TS(X) > TS(Ti)` OR `write_TS(X) > TS(Ti)`,
     rollback Ti (a later transaction already read/wrote a value this write would
     invalidate); else set `write_TS(X) = TS(Ti)`.
   - Worked example (T1=100, T2=200, T3=300): a sequence of reads/writes on A, B, C
     across the three transactions is checked step by step against these rules,
     ending in one rollback (`write_item(B)` by T1 — a later transaction, T3, had
     already read B, so T1's earlier-timestamped write is rejected).

2. **Multiversion Two-Phase Locking Using Certify Locks (MV2PL):** three lock modes
   — read, write, certify. Two versions of each item X: the **committed version** X
   (original) and a **local version** X′ (private copy made when a transaction
   acquires a write lock). Other transactions keep reading committed X while the
   writer works on X′ without affecting X. To commit, the writer must obtain a
   **certify lock**, which is incompatible with read locks — so commit is delayed
   until all current readers release their read locks. Once certify + all needed
   locks are held, X′ replaces X as the new committed version and the old X is
   discarded. Lock compatibility: Read-Read = compatible; Read-Write, Write-Write,
   anything-Certify = incompatible.

**Closing slide:** teases Lecture 10, Concurrency Control.
