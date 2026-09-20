---
type: module-notes
course: CSE321
module: 3
topics: "11–14"
title: CPU Scheduling
status: complete
date: 2026-06-18
exam: midterm
---

# CSE321 — Module 3: CPU Scheduling
**Topics 11–14 | Exam: Midterm | Silberschatz Ch.5 | OSTEP Ch.7–8**

> **This module is the primary math section for the midterm.** The key exam skill is: given a process table (arrival time, burst time), draw the Gantt chart and compute turnaround time, waiting time, and response time for every algorithm.

---

## Topic 11 — CPU Scheduling Basics: Burst Cycle, Criteria
**Ref: Silberschatz 5.1.1–5.1.3**

### CPU–I/O Burst Cycle
Processes alternate between:
- **CPU burst** — period of computation (using the CPU)
- **I/O burst** — period of waiting for I/O to complete

```
[CPU burst] → [I/O burst] → [CPU burst] → [I/O burst] → ... → [final CPU burst] → [terminate]
```

Process types:
- **CPU-bound:** few, long CPU bursts (e.g., scientific computing)
- **I/O-bound:** many, short CPU bursts (e.g., interactive programs, databases)

### When Does Scheduling Occur?
1. Process switches from **running → waiting** (non-preemptive)
2. Process switches from **running → ready** (preemptive — timer interrupt)
3. Process switches from **waiting → ready** (preemptive — I/O done)
4. Process **terminates** (non-preemptive)

If scheduling only at points 1 and 4: **non-preemptive (cooperative)**.
If scheduling at all four points: **preemptive**.

**Ready queue implementation** (slide-confirmed list): can be a FIFO queue, priority queue, tree, or unordered linked list — the data structure itself is not fixed by the algorithm; the algorithm determines the *selection policy* applied to it.

### Scheduling Criteria

| Criterion | Goal | Formula / Notes |
|---|---|---|
| **CPU utilization** | Maximize | Percentage of time CPU is busy (not idle) |
| **Throughput** | Maximize | Number of processes completed per unit time |
| **Turnaround time** | Minimize | Completion time − Arrival time |
| **Waiting time** | Minimize | Turnaround time − Burst time |
| **Response time** | Minimize | Time of first CPU response − Arrival time |

### Key Formulas

```
Turnaround Time (TAT)  = Completion Time (CT) − Arrival Time (AT)
Waiting Time (WT)      = Turnaround Time − Burst Time (BT)
                       = TAT − BT
Response Time (RT)     = Time of first CPU allocation − Arrival Time
Average TAT            = (sum of all TATs) / number of processes
Average WT             = (sum of all WTs) / number of processes
```

> **Exam note:** Response time ≠ Turnaround time. For non-preemptive algorithms, response time = waiting time (first CPU = start of execution). For preemptive (RR), a process may get CPU partway through, so RT ≠ WT.

---

## Topic 12 — FCFS and SJF
**Ref: Silberschatz 5.2**

### FCFS — First Come First Served

**Rule:** Processes served in order of arrival. Non-preemptive.

**Worked Example:**

| Process | AT | BT |
|---|---|---|
| P1 | 0 | 24 |
| P2 | 1 | 3 |
| P3 | 2 | 3 |

```
Gantt: [P1──────────────────────────────0..24][P2──24..27][P3──27..30]
              0                         24    27          30
```

| Process | AT | BT | CT | TAT | WT |
|---|---|---|---|---|---|
| P1 | 0 | 24 | 24 | 24 | 0 |
| P2 | 1 | 3 | 27 | 26 | 23 |
| P3 | 2 | 3 | 30 | 28 | 25 |
| **Avg** | | | | **26** | **16** |

**Convoy effect:** A long process (P1) causes short processes (P2, P3) to wait excessively. Average waiting time is high.

---

### SJF — Shortest Job First

**Rule:** Always pick the process with the shortest *next* CPU burst. Two versions:

> **Slide-confirmed framing:** SJF can be seen as a special case of Priority Scheduling (Topic 13) where the priority value **is** the predicted next CPU burst time (inverse relationship — shorter burst = higher priority). Useful if a question asks you to connect the two algorithms conceptually.

#### Non-preemptive SJF
- Once a process starts, it runs until its CPU burst finishes
- At each scheduling decision, pick shortest burst from ready queue

**Worked Example (non-preemptive SJF):**

| Process | AT | BT |
|---|---|---|
| P1 | 0 | 7 |
| P2 | 2 | 4 |
| P3 | 4 | 1 |
| P4 | 5 | 4 |

```
t=0: only P1 available → run P1 (BT=7), finishes at t=7
t=7: P2 (BT=4), P3 (BT=1), P4 (BT=4) in queue → pick P3 (shortest)
t=8: P2 (BT=4), P4 (BT=4) → tie → FCFS → pick P2
t=12: P4 → run until t=16

Gantt: [P1─0..7][P3─7..8][P2─8..12][P4─12..16]
```

| Process | AT | BT | CT | TAT | WT |
|---|---|---|---|---|---|
| P1 | 0 | 7 | 7 | 7 | 0 |
| P2 | 2 | 4 | 12 | 10 | 6 |
| P3 | 4 | 1 | 8 | 4 | 3 |
| P4 | 5 | 4 | 16 | 11 | 7 |
| **Avg** | | | | **8** | **4** |

#### Preemptive SJF = SRTF (Shortest Remaining Time First)
- At each new arrival, if the new process has a shorter remaining burst than the current process, preempt

**SRTF Example:**

| Process | AT | BT |
|---|---|---|
| P1 | 0 | 7 |
| P2 | 2 | 4 |
| P3 | 4 | 1 |
| P4 | 5 | 4 |

```
t=0: P1 starts (BT=7 remaining)
t=2: P2 arrives (BT=4). P1 remaining=5. 4 < 5 → preempt P1, run P2
t=4: P3 arrives (BT=1). P2 remaining=2. 1 < 2 → preempt P2, run P3
t=5: P4 arrives (BT=4). P3 remaining=0 (P3 finishes at t=5). P2 remaining=2, P1 remaining=5. Pick P2 (shortest)
t=7: P2 finishes. P1 remaining=5, P4 remaining=4. Pick P4
t=11: P4 finishes. P1 runs until t=16

Gantt: [P1─0..2][P2─2..4][P3─4..5][P2─5..7][P4─7..11][P1─11..16]
```

| Process | AT | BT | CT | TAT | WT |
|---|---|---|---|---|---|
| P1 | 0 | 7 | 16 | 16 | 9 |
| P2 | 2 | 4 | 7 | 5 | 1 |
| P3 | 4 | 1 | 5 | 1 | 0 |
| P4 | 5 | 4 | 11 | 6 | 2 |
| **Avg** | | | | **7** | **3** |

**SJF is optimal** for minimizing average waiting time. Problem: cannot know exact burst time in advance. Estimation uses **exponential average:**
```
τ(n+1) = α · t(n) + (1 − α) · τ(n)

where: τ(n+1) = next predicted burst
       t(n)   = actual n-th burst
       τ(n)   = previous prediction
       α      = weight (typically 0.5)
```

---

## Topic 13 — Priority Scheduling and Round Robin
**Ref: Silberschatz 5.3**

### Priority Scheduling

**Rule:** Each process has a priority integer. CPU goes to highest priority process. Convention varies: lower number = higher priority (UNIX), or higher number = higher priority — check the question.

- Can be **preemptive** (new higher-priority process preempts) or **non-preemptive** (current process finishes)
- **Starvation problem:** Low-priority processes may never execute if high-priority processes keep arriving
- **Solution — Aging:** Gradually increase the priority of waiting processes over time

**How priority is assigned (slide-confirmed factors):**

| Internal factors | External factors |
|---|---|
| Time limits | Importance of the process |
| Memory requirements | Funds paid for computer use |
| Number of open files | Sponsoring department |
| Ratio of I/O burst to CPU burst | |

**Worked Example (non-preemptive, lower number = higher priority, all arrive at t=0):**

| Process | AT | BT | Priority |
|---|---|---|---|
| P1 | 0 | 10 | 3 |
| P2 | 0 | 1 | 1 |
| P3 | 0 | 2 | 4 |
| P4 | 0 | 1 | 5 |
| P5 | 0 | 5 | 2 |

```
Order by priority: P2(1) → P5(2) → P1(3) → P3(4) → P4(5)
Gantt: [P2─0..1][P5─1..6][P1─6..16][P3─16..18][P4─18..19]
```

| Process | BT | CT | TAT | WT |
|---|---|---|---|---|
| P2 | 1 | 1 | 1 | 0 |
| P5 | 5 | 6 | 6 | 1 |
| P1 | 10 | 16 | 16 | 6 |
| P3 | 2 | 18 | 18 | 16 |
| P4 | 1 | 19 | 19 | 18 |
| **Avg** | | | **12** | **8.2** |

> **Gap flag:** this note only has a non-preemptive priority example. The slide's own worked example is **preemptive** (staggered arrival times causing a higher-priority process to interrupt one already running) — practice a preemptive priority trace separately tonight if time allows, since the note has no walkthrough for that mode despite priority scheduling explicitly supporting both.

---

### Round Robin (RR)

**Rule:** Preemptive. Each process gets a fixed **time quantum** q. If not done in q, it goes to the back of the ready queue. Ready queue treated as circular FIFO.

**Performance depends on quantum size:**
- q very large → degenerates to FCFS (processes finish within one quantum)
- q very small → high responsiveness but excessive context switches (overhead)
- Rule of thumb: q should be large enough that 80% of CPU bursts are shorter than q

**Worked Example (q = 4, all arrive at t=0):**

| Process | AT | BT |
|---|---|---|
| P1 | 0 | 24 |
| P2 | 0 | 3 |
| P3 | 0 | 3 |

```
Queue order: P1, P2, P3
t=0..4:   P1 runs 4ms (remaining: 20)
t=4..7:   P2 runs 3ms — FINISHES at t=7
t=7..10:  P3 runs 3ms — FINISHES at t=10
t=10..14: P1 runs 4ms (remaining: 16)
t=14..18: P1 runs 4ms (remaining: 12)
t=18..22: P1 runs 4ms (remaining: 8)
t=22..26: P1 runs 4ms (remaining: 4)
t=26..30: P1 runs 4ms — FINISHES at t=30

Gantt: [P1─0..4][P2─4..7][P3─7..10][P1─10..14][P1─14..18][P1─18..22][P1─22..26][P1─26..30]
```

| Process | AT | BT | CT | TAT | WT | RT |
|---|---|---|---|---|---|---|
| P1 | 0 | 24 | 30 | 30 | 6 | 0 |
| P2 | 0 | 3 | 7 | 7 | 4 | 4 |
| P3 | 0 | 3 | 10 | 10 | 7 | 7 |
| **Avg** | | | | **15.67** | **5.67** | **3.67** |

> **Exam note on Response Time with RR:** Response time = time of FIRST CPU slice. P1 gets CPU first at t=0, so RT=0. P2 first gets CPU at t=4, so RT=4. P3 first at t=7, so RT=7.

> **RR bound formula (slide-confirmed, was missing):** with n processes and quantum q, **no process waits more than (n−1)×q time units** before its next turn. Useful for a quick sanity-check or a direct "what's the maximum wait" question.

---

## Topic 14 — Multilevel Queue and MLFQ
**Ref: Silberschatz 5.3 | OSTEP Ch.8**

### Multilevel Queue (MLQ)

**Structure:** Multiple separate queues, each with its own scheduling algorithm. Processes are **permanently assigned** to one queue based on process type.

```
Priority ↑ │ Queue 0: System processes       [Real-time scheduler]
           │ Queue 1: Interactive processes  [Round Robin]
           │ Queue 2: Interactive editing    [Round Robin]
           │ Queue 3: Batch processes        [FCFS]
Priority ↓ │ Queue 4: Student processes      [FCFS]
```

**Scheduling between queues:** Fixed priority — a lower-priority queue only gets CPU if all higher-priority queues are empty. (Risk: starvation of batch processes if interactive queue never empties.)

Or: time slice between queues (e.g., 80% to interactive, 20% to batch).

---

### Multilevel Feedback Queue (MLFQ)

**Key difference from MLQ:** Processes can **move between queues** based on their behavior. New processes start at the top queue (highest priority). CPU-heavy processes get demoted; I/O-bound and short processes stay high.

**5 defining parameters of an MLFQ scheduler (slide-confirmed framing, distinct from the OSTEP rules below — both are legitimate ways the exam could ask about MLFQ):**
1. Number of queues
2. Scheduling algorithm used for each queue
3. Method used to **upgrade** (promote) a process to a higher queue
4. Method used to **demote** a process to a lower queue
5. Method used to determine **which queue a process enters** initially

**OSTEP Rules (memorize these):**

| Rule | Statement |
|---|---|
| **Rule 1** | If Priority(A) > Priority(B), A runs |
| **Rule 2** | If Priority(A) = Priority(B), A and B run in Round Robin |
| **Rule 3** | When a job enters, it is placed at highest priority |
| **Rule 4** | Once a job uses up its time allotment at a given level (regardless of how many times it gives up the CPU), its priority is reduced |
| **Rule 5** | After some time period S, move all jobs in the system to the topmost queue (**Priority Boost**) |

**Why Priority Boost (Rule 5)?**
Without it, CPU-bound jobs are permanently demoted to the bottom and starve. The boost periodically gives every process a chance to be scheduled.

**Example — MLFQ with 3 queues (Q0: q=8, Q1: q=16, Q2: FCFS):**

```
New job arrives → Q0 (q=8ms)
  If finishes in q=8 → done (it was short; stays short next time)
  If uses full 8ms → demote to Q1 (q=16ms)
    If finishes in 16ms → done
    If uses full 16ms → demote to Q2 (FCFS)
      Runs until completion (long CPU-bound job)

Every S=100ms: ALL jobs boosted back to Q0
```

**MLFQ goals:**
- Short jobs complete quickly (stay in high priority)
- I/O-bound interactive jobs stay responsive (they voluntarily yield CPU before quantum expires)
- Long CPU-bound jobs still get CPU but at low priority (fair via boost)

> **Slide-confirmed alternate promotion rule — important, differs from OSTEP Rule 5 above:** the professor's own worked examples ("Problem 1"/"Problem 2") promote a process **one queue level after each I/O operation**, rather than relying only on the periodic priority-boost-after-time-S rule. This is a more granular, per-I/O-event mechanic. Since both rules appear in course material (OSTEP boost vs. slide's per-I/O promotion), read any MLFQ question carefully for which rule it specifies — do not assume periodic boost is the only mechanism.

> **Slide inconsistency flag:** the deck's textbook-standard 3-queue example states **Queue 2 = FCFS**, but its own later worked MLFQ problems use **Queue 2 = Round Robin (quantum = 8)** instead. These give different bottom-queue behavior (run-to-completion vs. time-sliced). If a question doesn't specify the bottom queue's algorithm, don't assume — check what the question states, and default to whichever the question's own setup implies.

| Property | MLQ | MLFQ |
|---|---|---|
| Queue assignment | Permanent | Dynamic (based on behavior) |
| Starvation risk | Yes (without time slicing) | Mitigated by priority boost |
| Complexity | Low | High |
| Adaptability | None | Adapts to job behavior |

---

## Module 3 — CPU Scheduling Formula Sheet

```
TAT = CT − AT
WT  = TAT − BT
RT  = First CPU time − AT
Avg = (sum of metric) / n

SJF burst estimate: τ(n+1) = α·t(n) + (1−α)·τ(n)
RR max wait bound: (n−1) × q
```

### Algorithm Comparison

| Algorithm | Preemptive | Optimal for | Risk |
|---|---|---|---|
| FCFS | No | Simple queues | Convoy effect |
| SJF (non-preemptive) | No | Min avg WT | Need to know burst time |
| SRTF (preemptive SJF) | Yes | Min avg WT (best) | Starvation of long jobs |
| Priority | Both | Priority-based | Starvation (fix: aging) |
| Round Robin | Yes | Response time | High avg TAT; quantum tuning |
| MLQ | Both | Mixed workloads | Starvation at low queues |
| MLFQ | Yes | General purpose | Complex; needs tuning |
