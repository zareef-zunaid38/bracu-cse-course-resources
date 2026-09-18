# CSE370 Lecture 10: Concurrency Control — sidecar (read this, not the .pdf)

BRAC University CSE370: Database Systems slide deck, 12 slides. Clean digital slides
(mixed dark title cards and light content slides), fully legible — transcribed with
confidence. Continues Topic 24 (Concurrency Control) from Lecture 9's problem/isolation
framing, covering the actual lock-based enforcement mechanisms: 2PL, deadlock handling,
and graph-based protocols. Confirmed Quiz 4 syllabus (2026-09-07) alongside Lecture 9.

## Content

**Lock-based concurrency control — basic idea:** a lock controls access to a data
item; a transaction must acquire a lock before reading/writing it (ask permission
before use). Only compatible locks can be held simultaneously.

**Lock types:**
- **Shared lock (S-lock):** for reading only. Multiple transactions can hold S-locks
  on the same item simultaneously.
- **Exclusive lock (X-lock):** for read/write. Cannot be held simultaneously with any
  other lock (S or X) on the same item by a different transaction — blocks all others.
- Compatibility matrix: S-S = compatible; S-X, X-S, X-X = all incompatible.

**Two-Phase Locking (2PL):**
- **Growing phase:** transaction acquires all locks it needs; cannot release any
  lock during this phase.
- **Shrinking phase:** begins at the first lock release; can only release locks from
  here on, never acquire or upgrade new ones.
- **Lock point:** the moment the transaction obtains its *final* lock — the
  boundary between growing and shrinking.
- **Upgrading** (S→X): only allowed during the growing phase, and only if no other
  transaction holds an S-lock on that item.
- **Downgrading** (X→S): only allowed during the shrinking phase (reduces
  restriction, so generally permitted).
- **Worked example (3 transactions, 12 steps):** T1 locks A (X), reads, writes, then
  locks B (S) — this is T1's lock point (LP) — reads B, then unlocks both A and B.
  Meanwhile T2 tries to read A before T1's write completes → dirty read → T2 must
  roll back. T2 then legitimately locks A (X) after T1 releases it, reads/writes,
  unlocks. T3 tries to read A while T1 still holds it → dirty read → T3 rolls back;
  T3 later legitimately locks A (S) after both T1 and T2 release it.

**Categories of 2PL:**
- **Strict 2PL:** follows basic 2PL; all X-locks held until commit/abort; S-locks
  may be released earlier. Prevents dirty reads and cascading rollback.
- **Rigorous 2PL:** stronger than strict — *all* locks (S and X) held until commit;
  released only after the transaction finishes.
- **Conservative 2PL:** locks all required items upfront before starting; if not
  all available, the transaction waits (no partial locking). Deadlock-free and
  ensures serializability, but reduces concurrency and has low practicality.

**Deadlock handling:**
- **Deadlock:** transactions block each other permanently because each holds a
  resource the other needs (T1 has A, needs B; T2 has B, needs A → circular wait).
  Problems caused: transactions stay blocked, system throughput drops, resources
  sit locked unused, in severe cases the system stalls.
- **Necessary conditions:** Hold and Wait (holding one resource while requesting
  another held elsewhere), No Preemption (a resource can't be forcibly taken, only
  voluntarily released), Circular Wait (a cycle of transactions each waiting on the
  next).
- **Deadlock avoidance:** design so deadlock can't occur — (1) always lock resources
  in the same fixed order (e.g. always A before B) so two transactions can't block
  each other in opposite order; (2) use smaller-granularity locks and appropriate
  isolation levels (row locks instead of table locks) — reduces but doesn't
  eliminate risk.
- **Deadlock detection — Wait-For Graph:** each transaction is a node; an edge means
  one transaction is waiting for another. A cycle in the graph (e.g. T1→T2 needs B,
  T2→T1 needs A) confirms a deadlock. Simple, suited to small/medium systems.
- **Deadlock prevention schemes** (assume T1 older, T2 younger, by timestamp):
  - **Wait-Die (non-preemptive):** an *older* transaction may wait for a younger
    one's resource; a *younger* transaction requesting a resource held by an older
    one is aborted and restarted. Avoids cycles because younger transactions never
    wait on older ones.
  - **Wound-Wait (preemptive):** an *older* transaction can force-abort ("wound") a
    younger one holding a resource it needs; a *younger* transaction requesting an
    older one's resource must wait. Avoids deadlock because older transactions are
    never blocked by younger ones.
  - Both examples worked with T1 holding Account A, T2 holding Account B, and each
    then requesting the other's account.
  - **Important:** aborted/restarted transactions keep their *original* timestamp on
    restart, to reduce starvation risk.
- **Deadlock recovery:** once detected, pick a victim transaction (lowest rollback
  cost) and roll it back to release its locks.
  - **Total rollback:** restart the victim from the beginning.
  - **Partial rollback:** undo only as much as needed to break the cycle.
  - **Starvation problem:** the same transaction might keep getting picked as
    victim. **Solution:** track each transaction's rollback count, increase its
    priority after each rollback, and factor in both age and rollback count when
    choosing a victim — avoid repeatedly picking the same one.
- **Timeout-based handling:** a transaction waits only a fixed time; if exceeded, it
  is rolled back. Advantage: simple to implement, breaks real deadlocks. Limitation:
  may roll back transactions that weren't actually deadlocked, picking a good
  timeout value is hard, and starvation can still occur.
- **Bottom line:** deadlocks can't always be fully avoided, so real systems combine
  prevention + detection + recovery to keep the system moving without transactions
  stuck forever.

**Graph-Based Concurrency Control (alternative to 2PL):** defines a fixed access
order for data items instead of a growing/shrinking lock discipline.
- Data items = nodes; allowed access order = directed edges. If A→B, any transaction
  touching both must access A before B. The access-order graph itself must be
  acyclic (a cycle like A→B→C→A is disallowed; a simple chain A→B→C is fine).
- **Properties:** conflict-serializable (following the fixed order guarantees this);
  deadlock-free (acyclic order prevents circular wait); may still cause cascading
  rollback (recoverability isn't guaranteed); limitation — the access order must be
  fixed in advance.
- **Tree Protocol** (a simple graph-based protocol): only X-locks are used (no
  S-locks). The first lock in a transaction can be on any data item; every
  subsequent lock must be on a child of an item the transaction already holds a
  lock on (i.e., you must hold the parent before locking a child). Unlocking is
  allowed at any time (no strict growing/shrinking phases like 2PL). Once a data
  item is unlocked by a transaction, that transaction may never relock it.
  - Worked example (tree A→{B,C}, B→{D,E}, C→G): T1 locks A, then B (child of A),
    then D (child of B) — all valid, following the parent-first rule. T2 locks C
    (a valid first lock, as root's child) then G (child of C) — valid — but then
    T2's attempt to lock D is **rejected**, because T2 never held D's parent B.

**Closing slide:** teases Lecture 11, Query Processing.
