---
type: module-notes
course: CSE321
module: 4
topics: "15–19"
title: Synchronization and Deadlock
status: complete
date: 2026-06-18
exam: midterm
---

# CSE321 — Module 4: Synchronization and Deadlock
**Topics 15–19 | Exam: Midterm | Silberschatz Ch.6–7 | OSTEP Ch.28–32**

---

## Topic 15 — Race Conditions and the Critical Section Problem
**Ref: Silberschatz 6.1, 6.2**

### Race Condition
A **race condition** occurs when the outcome of a computation depends on the relative timing of two or more threads/processes accessing shared data.

**Classic example — two threads incrementing a counter:**
```
Thread 1                    Thread 2
READ counter (= 5)
                            READ counter (= 5)
counter = 5 + 1 = 6
WRITE counter = 6
                            counter = 5 + 1 = 6
                            WRITE counter = 6
Final value: 6   ← WRONG (should be 7)
```
The problem: `counter++` is not atomic — it compiles to three instructions (load, add, store). An interleaving that writes both results based on the same old value causes lost updates.

### Critical Section
A **critical section** is a code segment in which a process accesses shared resources (shared variables, files, I/O). Only one process may execute in its critical section at a time.

**Structure of a solution:**

```
do {
    // Entry section — request permission to enter CS
    [critical section]   // access shared data
    // Exit section — signal CS is vacated
    [remainder section]  // rest of process logic
} while (true);
```

### Three Requirements for a Correct CS Solution

| Requirement | Definition |
|---|---|
| **1. Mutual Exclusion** | If process Pi is in its CS, no other process may be in its CS |
| **2. Progress** | If no process is in the CS and some wish to enter, selection of the entering process cannot be postponed indefinitely. Only processes NOT in their remainder section participate in this decision |
| **3. Bounded Waiting** | There exists a bound on the number of times other processes can enter the CS after a process has made a request and before that request is granted |

> **Slide-verification note:** Peterson's Solution below is **not present anywhere in the 42-page slide deck** — it's Silberschatz-textbook-only. Keep it for moderate-difficulty/textbook-sourced questions, but it's lower priority than the hardware and semaphore content that follows, which the slides do cover directly.

### Peterson's Solution (2-process software solution)
```c
int turn;          // whose turn it is
bool flag[2];      // flag[i] = true means Pi wants to enter CS

// Process Pi (i = 0 or 1, j = 1 − i):
flag[i] = true;
turn = j;
while (flag[j] && turn == j)
    ; // busy wait
// --- CRITICAL SECTION ---
flag[i] = false;
// --- REMAINDER ---
```
- Satisfies all 3 requirements theoretically
- **Not reliable on modern hardware** without memory barriers (CPUs reorder instructions)

---

## Topic 16 — Hardware Synchronization: Test-and-Set, Compare-and-Swap
**Ref: Silberschatz 6.4.2**

Hardware provides **atomic** instructions — they execute without interruption from any other instruction.

### Disabling Interrupts (slide-confirmed, was missing)
On a **single-processor** system, the simplest hardware-based CS solution: disable all interrupts before entering the critical section, re-enable them on exit. While interrupts are disabled, no context switch can occur, so mutual exclusion is guaranteed by construction.
- **Limitation:** does not work on multiprocessor systems (disabling interrupts on one core doesn't stop another core from entering its own CS on shared data). Also risky if the CS is long — the system becomes unresponsive to all other interrupts during that time.

### Test-and-Set (TAS)

```c
boolean test_and_set(boolean *target) {
    boolean rv = *target;   // read old value
    *target = true;         // always set to true
    return rv;              // return old value
}
```

**Usage — spinlock with TAS:**
```c
boolean lock = false;  // false = available

do {
    while (test_and_set(&lock))
        ;              // spin: wait until lock was false (returns false, sets to true)
    // --- CRITICAL SECTION ---
    lock = false;      // release
    // --- REMAINDER ---
} while (true);
```

- When `lock = false`: TAS returns false (old value) and sets lock to true → process enters CS
- When `lock = true`: TAS returns true (old value) and sets lock to true again → process spins

**Problem with basic TAS:** Does not guarantee bounded waiting. A process could spin indefinitely if others keep acquiring the lock.

---

### Compare-and-Swap (CAS)

```c
int compare_and_swap(int *value, int expected, int new_value) {
    int temp = *value;
    if (*value == expected)     // only update if current value matches expected
        *value = new_value;
    return temp;                // return old value
}
```

**Usage:**
```c
int lock = 0;  // 0 = available, 1 = locked

while (compare_and_swap(&lock, 0, 1) != 0)
    ;           // spin: only succeeds (returns 0) when lock was 0
// --- CRITICAL SECTION ---
lock = 0;       // release
```

CAS is more general than TAS:
- TAS: always writes `true`
- CAS: only writes if current value matches expected — useful for **lock-free data structures** (e.g., atomic counters, lock-free queues)

| | TAS | CAS |
|---|---|---|
| Writes unconditionally | Yes | No — only if current = expected |
| Returns | Old value | Old value |
| Use | Simple spinlock | Lock-free structures, more complex synchronization |

> **Exam note:** Be able to write the TAS and CAS function bodies from memory and show how they are used as a spinlock. Know that both are **hardware atomic** instructions.

---

## Topic 17 — Mutex Locks and Semaphores
**Ref: Silberschatz 6.5, 6.6**

### Mutex Lock

A **mutex (mutual exclusion lock)** is a binary lock with two operations:
- `acquire()` — take the lock; block if already held
- `release()` — release the lock; wake a waiting process

```c
acquire() {
    while (!available)
        ;          // busy wait (spinlock implementation)
    available = false;
}

release() {
    available = true;
}
```

**Spinlock vs blocking mutex:**
- **Spinlock:** thread busy-waits (loops). No context switch overhead. Wastes CPU. Best when CS is very short and on multi-core systems.
- **Blocking mutex:** thread is put to sleep (context switch). No CPU waste while waiting. Better when CS may be long.

---

### Semaphore

A **semaphore** S is an integer variable, accessed only through two **atomic** operations:

```
wait(S):      signal(S):
  S--;          S++;
  if S < 0:     if S <= 0:
    block();      wake one waiting process;
```

(Silberschatz also uses `P()` for wait and `V()` for signal — same operations.)

**Low-level implementation avoiding busy-waiting (slide-confirmed, was missing):** rather than spinning, a semaphore can be implemented with a waiting list so a blocked process is put to sleep instead of wasting CPU:
```c
typedef struct {
    int value;
    struct process *list;  // list of processes waiting on this semaphore
} semaphore;

wait(semaphore *S) {
    S->value--;
    if (S->value < 0) {
        add this process to S->list;
        block();            // sleep — remove from CPU
    }
}

signal(semaphore *S) {
    S->value++;
    if (S->value <= 0) {
        remove a process P from S->list;
        wakeup(P);           // move P back to ready queue
    }
}
```
> **Starvation cause (slide-confirmed, quotable fact):** starvation can occur if processes are removed from the semaphore's waiting list in **LIFO order** — a process at the back of a stack-ordered list may never be selected while newer arrivals keep jumping ahead of it. FIFO removal avoids this.

**Two types:**

| Type | S value | Use |
|---|---|---|
| **Binary semaphore** (mutex) | 0 or 1 | Mutual exclusion (same as mutex lock) |
| **Counting semaphore** | Any non-negative integer | Controlling access to a pool of N resources |

**Counting semaphore example:** S initialized to N = number of available resource instances. Each `wait()` decrements (acquires one resource); each `signal()` increments (releases one resource). S < 0 means |S| processes are blocked waiting.

**Semaphore for ordering (signaling):**
```c
// Ensure statement B in P2 executes only after statement A in P1:
// S initialized to 0

// P1:
  execute A;
  signal(S);    // S becomes 1 (or wakes P2 if blocked)

// P2:
  wait(S);      // blocks until S > 0
  execute B;
```

### Semaphore Problems: Deadlock and Starvation

```
S = 1, Q = 1

P0:  wait(S)   wait(Q)   ...   signal(S)   signal(Q)
P1:  wait(Q)   wait(S)   ...   signal(Q)   signal(S)
```

If P0 acquires S and P1 acquires Q simultaneously:
- P0 waits for Q (held by P1)
- P1 waits for S (held by P0)
- **Deadlock** — neither can proceed

**Priority inversion:** High-priority process H waits for a lock held by low-priority process L. Medium-priority process M preempts L (because M > L), preventing L from releasing the lock. H is effectively blocked by M.

**Solution — Priority Inheritance:** L temporarily inherits H's priority while holding the lock, so M cannot preempt L.

---

## Topic 18 — Classical Synchronization Problems
**Ref: Silberschatz 6.8.1 | OSTEP Ch.30–31**

### 1. Bounded Buffer (Producer-Consumer)

**Scenario:** Producer generates items into a buffer of size N. Consumer takes items out. Both run concurrently.

> **Slide-confirmed motivating example — why semaphores are needed (was missing):** an "Inadequate Solution" using a plain shared `itemCount` variable with `sleep()`/`wakeup()` calls (no semaphore) is shown first. The race: producer checks `if (itemCount == BUFFER_SIZE) sleep()`, but between that check and the actual sleep call, the consumer can remove an item and call `wakeup()` — which is lost because the producer wasn't asleep yet. The producer then sleeps forever even though space is now free (**lost wakeup**). This is the exact problem semaphores solve, since `wait()`/`signal()` are atomic and can't have this check-then-act gap.

**Semaphores** (note: slides name these `mutex`, `countFull`, `countEmpty` — functionally identical to `mutex`/`full`/`empty` below, just different variable names; recognize either if a question quotes slide naming):
```
mutex  = 1   (binary — protects buffer access)
empty  = N   (counting — tracks empty slots; producer waits when 0)
full   = 0   (counting — tracks full slots; consumer waits when 0)
```

```c
// PRODUCER:
do {
    produce item;
    wait(empty);     // decrement empty slot count; block if 0
    wait(mutex);     // lock buffer
    add item to buffer;
    signal(mutex);   // unlock buffer
    signal(full);    // increment full slot count
} while (true);

// CONSUMER:
do {
    wait(full);      // decrement full slot count; block if 0
    wait(mutex);     // lock buffer
    remove item from buffer;
    signal(mutex);   // unlock buffer
    signal(empty);   // increment empty slot count
    consume item;
} while (true);
```

> **Critical:** `wait(empty)` BEFORE `wait(mutex)` in producer, and `wait(full)` BEFORE `wait(mutex)` in consumer. Reversing the order causes deadlock.

---

### 2. Readers-Writers Problem

**Scenario:** A shared database. Multiple readers may read simultaneously. A writer needs exclusive access (no readers or other writers).

**Key variables:**
```
rw_mutex  = 1    (exclusive access for writers)
mutex     = 1    (protects read_count)
read_count = 0   (number of active readers)
```

```c
// WRITER:
wait(rw_mutex);
  [write data]
signal(rw_mutex);

// READER:
wait(mutex);
  read_count++;
  if (read_count == 1)
    wait(rw_mutex);   // first reader locks out writers
signal(mutex);

  [read data]

wait(mutex);
  read_count--;
  if (read_count == 0)
    signal(rw_mutex); // last reader unlocks for writers
signal(mutex);
```

**Problem:** If readers keep arriving, writers may starve (first-reader-writer problem). Second variant (second readers-writers): once a writer is waiting, no new readers are admitted.

---

### 3. Dining Philosophers Problem

**Scenario:** N philosophers sit at a round table. N chopsticks, one between each pair. A philosopher needs both left and right chopstick to eat.

**Naive solution (causes deadlock):**
```c
wait(chopstick[i]);           // pick up left
wait(chopstick[(i+1) % N]);   // pick up right
  [eat]
signal(chopstick[i]);
signal(chopstick[(i+1) % N]);
```
If every philosopher picks up their left chopstick simultaneously, all block waiting for the right → **circular wait → deadlock**.

**Fix 1 — Allow only N−1 philosophers to sit at once:**
```
sem_t seats;  // initialized to N-1
wait(seats) before picking up chopsticks; signal(seats) after eating
```

**Fix 2 — Asymmetric:** Odd-numbered philosophers pick left then right; even-numbered pick right then left. Breaks circular wait.

**Fix 3 — Only pick both chopsticks if both available** (using a monitor or waiter semaphore — not just pairwise semaphores).

---

## Topic 19 — Deadlock: Necessary Conditions and Resource Allocation Graph
**Ref: Silberschatz 7.1**

### Four Necessary Conditions for Deadlock

ALL four must hold simultaneously for deadlock to occur. Breaking any one prevents deadlock.

| Condition | Definition |
|---|---|
| **1. Mutual Exclusion** | At least one resource must be held in non-shareable mode (only one process at a time can use it) |
| **2. Hold and Wait** | A process holds at least one resource and is waiting to acquire additional resources held by other processes |
| **3. No Preemption** | Resources cannot be forcibly taken from a process; a resource is only released voluntarily by the process holding it |
| **4. Circular Wait** | There exists a set {P0, P1, ..., Pn} such that P0 waits for P1, P1 waits for P2, ..., Pn waits for P0 |

> **Exam note:** Questions often ask: "which condition does solution X break?" Know which deadlock prevention strategy targets which condition.

---

> **Slide-verification note:** the slide deck ends at "Necessary Conditions for Deadlock" (above) and **never introduces the Resource Allocation Graph** — the RAG section below is entirely Silberschatz-textbook content. It's still in scope per the topic map, and the PYQ pattern analysis found zero RAG appearances across 13 previous midterm sittings — know the notation and cycle rule well enough not to lose an easy mark, but it is the lowest-priority item in this module if time is short tonight.

### Resource Allocation Graph (RAG)

**Nodes:**
- Circles = processes (P1, P2, ...)
- Rectangles = resource types (R1, R2, ...) — dots inside = instances of that resource

**Edges:**
- **Request edge:** P → R (process P is requesting an instance of R; arrow points to resource)
- **Assignment edge:** R → P (an instance of R is assigned to process P; arrow points to process)

```
Request edge:    Pi ──────────────► Rj
Assignment edge: Pi ◄────────────── Rj
```

**Deadlock detection via RAG:**

| Situation | Deadlock? |
|---|---|
| No cycle | No deadlock possible |
| Cycle exists AND each resource type has **exactly one instance** | Deadlock **guaranteed** |
| Cycle exists AND resource types have **multiple instances** | Deadlock **possible but not certain** — need Banker's algorithm to confirm |

**Example RAG — Deadlock:**
```
P1 ──request──► R1 (1 instance, assigned to P2)
P2 ──request──► R2 (1 instance, assigned to P3)
P3 ──request──► R1

Cycle: P1→R1→P2→R2→P3→R1→...
Each resource has 1 instance → Deadlock.
```

**Example RAG — Cycle but No Deadlock:**
```
R1 has 2 instances: assigned to P1 and P3
P2 requests R1

Cycle exists (P1→R1→P2→... via P2 waiting), but P3 can release R1,
unblocking P2. No deadlock.
```

---

## Module 4 — Quick Review

| # | Topic | Must Know |
|---|---|---|
| 15 | Race conditions & CS | Race condition cause; 3 CS requirements (ME, Progress, Bounded Waiting) |
| 16 | TAS & CAS | Function body of each; spinlock usage; what "atomic" means |
| 17 | Mutex & Semaphore | wait()/signal() semantics; binary vs counting; ordering pattern |
| 18 | Classical problems | Semaphore assignments for Bounded Buffer; Readers-Writers read_count logic; Dining Philosophers deadlock + fixes |
| 19 | Deadlock & RAG | 4 necessary conditions; RAG notation; cycle → deadlock only with single-instance resources |

### Semaphore Quick Reference

```
Bounded Buffer:   mutex=1, empty=N, full=0
Readers-Writers:  rw_mutex=1, mutex=1, read_count=0
Ordering (A→B):   S=0; P1: do A, signal(S); P2: wait(S), do B
```
