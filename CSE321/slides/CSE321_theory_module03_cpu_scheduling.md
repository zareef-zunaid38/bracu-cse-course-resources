---
source: CSE321_theory_module03_cpu_scheduling.pptx (the original, included in this repo)
fidelity: the summary above the divider was rewritten from the deck on 2026-09-20 after an audit found errors in the earlier version; it is still a condensed paraphrase and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 4 embedded image(s); informational images are described in the summary)
source-mtime: 2025-07-12T20:18:42
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Module 03: CPU Scheduling

Lecture slides (27) on CPU scheduling: basic concepts, the scheduler and its decision points, scheduling criteria, and the algorithms FCFS, SJF/SRTF, priority, round robin, multilevel queue and multilevel feedback queue, each with worked examples. Sections in the deck: Basic concepts (1–5), FCFS (6–7), SJF (8–10), Priority (11–13), Round Robin (14–18), Multilevel Queue and Multilevel Feedback Queue (19–27).
Pptx last modified: 12-07-2025.
Rewritten 2026-09-20 directly from the deck (an audit found the earlier summary's section numbers and content unreliable). **A "Dispatcher" section was not in this deck.** Gantt charts and figures were read from renders of slides 7, 10, 13, 16, 17, 18, 21, 23, 24, 26 and 27.

## Content

### Slides 2–5: Basic concepts, scheduler, criteria
- **Slide 2:** maximum CPU utilization comes from multiprogramming; the continuous cycle — a process waits (I/O), the OS takes the CPU away and gives it to another process, and so on. *CPU–I/O burst cycle:* process execution alternates CPU execution and I/O wait.
- **Slide 3, CPU scheduler:** selects from the ready queue and allocates the CPU to one process (the queue may be a FIFO queue, priority queue, tree or unordered linked list). Decisions may occur when a process (1) switches running → waiting (I/O request), (2) switches running → ready (e.g. interrupt), (3) switches waiting → ready (e.g. I/O completion), (4) terminates. Scheduling under 1 and 4 is nonpreemptive; all other scheduling is preemptive. Preemption raises issues with access to shared data, preemption while in kernel mode, and interrupts during crucial OS activities.
- **Slide 4, criteria:** *CPU utilization* — keep the CPU as busy as possible; *throughput* — processes completed per time unit; *turnaround time* — time from submission to completion (the sum of waiting to get into memory, waiting in the ready queue, executing on the CPU and doing I/O); *waiting time* — time spent in the ready queue; *response time* — time from submitting a request until the first response is produced (not output), for time-sharing.
- **Slide 5, optimization:** maximize CPU utilization and throughput; minimize turnaround, waiting and response time.

### Slides 6–7: First-Come, First-Served (FCFS)
- **Slide 7 example** — processes (arrival, burst): P1 (0, 8), P2 (1, 4), P3 (2, 9), P4 (3, 5). Gantt chart: P1 0–8, P2 8–12, P3 12–21, P4 21–26. The slide also traces the ready queue over time (t=0: P1(8); t=1: P2(4); t=2: P2(4) P3(9); t=3: P2(4) P3(9) P4(5); t=8: P3(9) P4(5); t=12: P4(5); t=21: empty; t=26: empty).

### Slides 8–10: Shortest-Job-First (SJF)
- **Slide 9:** associate with each process the length of its next CPU burst and schedule the shortest. Two schemes: *non-preemptive* (once given the CPU it runs to the end of its burst) and *preemptive* (if a new process arrives with a burst shorter than the remaining time of the running process, preempt) — the latter is called Shortest-Remaining-Time-First (SRTF). SJF is optimal: it gives the minimum average waiting time for a given set of processes.
- **Slide 10, SRTF example** (same four processes as FCFS). Gantt chart: P1 0–1, P2 1–5, P4 5–10, P1 10–17, P3 17–26 (ready-queue trace shown: t=1 P1(7); t=2 P1(7) P3(9); t=3 P1(7) P3(9) P4(5); t=5 P1(7) P3(9); t=10 P3(9); t=17 empty; t=26 empty).

### Slides 11–13: Priority scheduling
- **Slide 12:** an integer priority number is associated with each process; the CPU goes to the highest priority (smallest integer = highest priority); preemptive or nonpreemptive; SJF is priority scheduling where priority is the inverse of the predicted next CPU burst. Priority may be internal (factors: time limit, memory requirements, number of open files, etc.) or external (importance of the process, type and amount of funds paid for computer use, sponsoring department, etc.).
- **Slide 13, preemptive example** — (arrival, burst, priority): P1 (0, 8, 3), P2 (1, 4, 1), P3 (2, 9, 4), P4 (3, 5, 5). Gantt chart: P1 0–1, P2 1–5, P1 5–12, P3 12–21, P4 21–26. Problem: **starvation** — low-priority processes may never execute. Solution: **aging** — raise a process's priority as time passes.

### Slides 14–18: Round Robin (RR)
- **Slide 15:** each process gets a small unit of CPU time, the time quantum q (usually 10–100 ms); after it elapses the process is preempted and added to the end of the ready queue. With n processes in the ready queue each gets 1/n of the CPU in chunks of at most q, and no process waits more than (n−1)q time units. A timer interrupts every quantum to schedule the next process. Performance: q large ⇒ FIFO; q small ⇒ q must be large relative to the context-switch time or overhead is too high.
- **Slide 16, q = 4 example** (same four processes as FCFS). Gantt chart: P1 0–4, P2 4–8, P3 8–12, P4 12–16, P1 16–20, P3 20–24, P4 24–25, P3 25–26 (ready-queue trace shown on the slide).
- **Slide 17, time quantum and context-switch time:** a process needing 10 time units — quantum 12 → 0 context switches; quantum 6 → 1; quantum 1 → 9.
- **Slide 18:** graph of average turnaround time against time quantum (1–7) for processes with times P1 = 6, P2 = 3, P3 = 1, P4 = 7; the curve is not monotonic (roughly 11.0 at q=1, peaking near 12.2 at q=5, about 10.5 at q=6–7). Rule of thumb: 80% of CPU bursts should be shorter than the quantum.

### Slides 19–27: Multilevel queue and multilevel feedback queue
- **Slide 20, multilevel queue:** processes are classified into groups, e.g. foreground (interactive) and background (batch), with different response-time needs; the ready queue is partitioned into separate queues, each with its own algorithm (foreground: RR; background: FCFS). Scheduling between the queues: fixed-priority preemptive (serve all foreground first; possibility of starvation) or time slicing (each queue gets a share of CPU time, e.g. 80% to the foreground RR queue and 20% to the background FCFS queue). **Slide 21 figure:** queues from highest to lowest priority — system processes, interactive processes, interactive editing processes, batch processes, student processes.
- **Slide 22, multilevel feedback queue:** a process can move between queues — one that uses too much CPU time moves to a lower-priority queue, one that waits too long in a low-priority queue may move up. The scheduler is defined by: the number of queues; the scheduling algorithm of each queue; the method to upgrade a process; the method to demote a process; the method to decide which queue a new/returning process enters.
- **Slide 23, example:** three queues — Q0 RR with quantum 8 ms, Q1 RR with quantum 16 ms, Q2 FCFS. A new job enters Q0 and gets 8 ms; if unfinished it moves to Q1 and gets 16 more ms; if still unfinished it is preempted and moved to Q2. (Figure: three stacked queues labelled quantum = 8, quantum = 16, FCFS, each feeding the next.)
- **Slides 24–25, Problem 1:** burst times P1 = 12, P2 = 8, P3 = 18; Queue 0 RR (quantum 2), Queue 1 RR (quantum 4), Queue 2 RR (quantum 8). CPU Gantt chart: P1 0–2, P2 2–4, P3 4–6, P1 6–10, P2 10–14, P3 14–18, P1 18–24, P2 24–26, P3 26–34, P3 34–38. Waiting time: P1 = (0−0)+(6−2)+(18−10) = 12; P2 = (2−0)+(10−4)+(24−14) = 18; P3 = (4−0)+(14−6)+(26−18)+(34−34) = 20; average = (12+18+20)/3 = 16.67. Response time: P1 = 0, P2 = 2, P3 = 4; average 2. Turnaround time: P1 = 24, P2 = 26, P3 = 38; average 29.33.
- **Slides 26–27, Problem 2 (with I/O):** same three queues (quanta 2, 4, 8) plus an I/O queue; processes (burst, arrival, I/O): P1 (12, 0, 6 total: a 2 s I/O after 4 s of CPU and a 4 s I/O after 9 s of CPU), P2 (8, 1, 1: a 1 s I/O after 5 s of CPU), P3 (18, 4, none), P4 (7, 20, 5: a 5 s I/O after 4 s of CPU). A process is promoted one level after an I/O operation. The slides show the per-queue traces, the CPU Gantt chart with I/O markers (ends at 45) and an I/O-operations Gantt chart; those charts are drawings — see the PDF pages for the cell-by-cell schedule. Results on slide 27: waiting time P1 = (0−0)+(6−2)+(11−10)+(19−13)+(27−26)+(29−29) = 12, P2 = (2−1)+(8−4)+(13−12)+(24−15) = 15, P3 = (4−4)+(15−6)+(30−19)+(41−38) = 23, P4 = (22−20)+(25−24)+(38−32)+(40−40) = 9, average 14.75; response time P1 = 0, P2 = 1, P3 = 0, P4 = 2, average 0.75; turnaround time P1 = 30, P2 = 24, P3 = 41, P4 = 21, average 29.

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets; list numbering, formatting and images are not reproduced -->

<!-- slide 1 -->
### OPERATING SYSTEMS CPU Scheduling

<!-- slide 2 -->
### Basic Concepts
Maximum CPU utilization obtained with multiprogramming
Continuous Cycle :
  one process has to wait (I/O)
  Operating system takes the CPU away
  Give CPU to another process
  This pattern continues
CPU–I/O Burst Cycle : Process execution consists of a cycle of CPU execution and I/O wait
Processor
Schedule
Processes in Ready Queue

<!-- slide 3 -->
### CPU Scheduler
Selects from among the processes in ready queue, and allocates the CPU to one of them
  FIFO queue
  Priority queue
  Tree
  Unordered linked-list
CPU scheduling decisions may take place when a process:
  1. Switches from running to waiting state (I/O request)
  2. Switches from running to ready state (e.g. when interrupt occurs)
  3. Switches from waiting to ready (e.g. at completion of I/O)
  Terminates
Scheduling under 1 and 4 is nonpreemptive
All other scheduling is preemptive
  Consider access to shared data
  Consider preemption while in kernel mode
  Consider interrupts occurring during crucial OS activities

<!-- slide 4 -->
### Scheduling Criteria
CPU utilization – keep the CPU as busy as possible
Throughput – # of processes that complete their execution per time unit
Turnaround time – amount of time to execute a particular process -- the interval from the time of submission of a process to the time of the completion. -- sum of the periods spent waiting to get into memory, waiting in the ready queue, executing on the CPU, doing I/O
Waiting time – amount of time a process has been waiting in the ready queue
Response time – amount of time it takes from when a request was submitted until the first response is produced, not output (for time-sharing environment)

<!-- slide 5 -->
### Scheduling Algorithm Optimization Criteria
Max CPU utilization
Max throughput
Min turnaround time
Min waiting time
Min response time

<!-- slide 6 -->
### OPERATING SYSTEMS CPU Scheduling Algorithms - First Come First Serve (FCFS)

<!-- slide 7 -->
### First-Come, First-Served (FCFS) Scheduling
The Gantt Chart for the schedule is:
| Process | Arrival Time | Burst Time |
| P1 | 0 | 8 |
| P2 | 1 | 4 |
| P3 | 2 | 9 |
| P4 | 3 | 5 |
P1
P2
P3
P4
0
8
12
21
26
0
8
12
21
26
Ready Queue
P1(8)
P3(9)
P4(5)
P4(5)
Empty
Empty
1
2
3
P2(4)
P2(4)
P3(9)
P2(4)
P3(9)
P4(5)

<!-- slide 8 -->
### OPERATING SYSTEMS CPU Scheduling Algorithms - Shortest Job First (SJF)

<!-- slide 9 -->
### Shortest-Job-First (SJF) Scheduling
Associate with each process the length of its next CPU burst. Use these lengths to schedule the process with the shortest time
Two schemes:
  Non-preemptive – once CPU given to the process it cannot be preempted until completes its CPU burst
  preemptive – if a new process arrives with CPU burst length less than remaining time of current executing process, preempt. This scheme is known as the Shortest-Remaining-Time-First (SRTF)
SJF is optimal – gives minimum average waiting time for a given set of processes

<!-- slide 10 -->
### Example of Shortest-remaining-time-first
P1
P2
P4
P1
P3
0
1
5
10
17
26
| Process | Arrival Time | Burst Time |
| P1 | 0 | 8 |
| P2 | 1 | 4 |
| P3 | 2 | 9 |
| P4 | 3 | 5 |
Gantt Chart:
0
1
5
10
17
26
Ready Queue
P1(8)
P2(4)
P1(7)
P3(9)
P3(9)
Empty
Empty
2
P1(7)
P3(9)
3
P1(7)
P3(9)
P4(5)
0
Empty
1
P1(7)

<!-- slide 11 -->
### OPERATING SYSTEMS CPU Scheduling Algorithms - Priority Scheduling

<!-- slide 12 -->
### Priority Scheduling
A priority number (integer) is associated with each process
The CPU is allocated to the process with the highest priority
(smallest integer ≡ highest priority)
  Preemptive
  Nonpreemptive
SJF is priority scheduling where priority is the inverse of predicted next CPU burst time
Priority can be defined either internally or externally.
  Factors for internal priority assignment:
    Time limit, memory requirements, the number or open files etc.
  Factors for external priority assignment:
    Importance of the process, the type and amount of funds being paid for computer use, department sponsoring works etc.

<!-- slide 13 -->
### Example of Priority Scheduling (Preemptive)
Priority scheduling Gantt Chart
Problem ≡ Starvation – low priority processes may never execute
Solution ≡ Aging – as time progresses increase the priority of the process
| Process | Arrival Time | Burst Time | Priority |
| P1 | 0 | 8 | 3 |
| P2 | 1 | 4 | 1 |
| P3 | 2 | 9 | 4 |
| P4 | 3 | 5 | 5 |
P1
P2
P1
P3
P4
0
1
5
12
21
26
Ready Queue
0
1
5
12
21
26
P1(8)
P2(4)
P3(9)
P4(5)
P4(5)
Empty
Empty
2
P1(7)
3
P1(7)
P3(9)
P4(5)
P3(9)
0
Empty
1
P1(7)

<!-- slide 14 -->
### OPERATING SYSTEMS CPU Scheduling Algorithms - Round Robin (RR)

<!-- slide 15 -->
### Round Robin (RR)
Each process gets a small unit of CPU time (time quantum q), usually 10-100 milliseconds. After this time has elapsed, the process is preempted and added to the end of the ready queue.
If there are n processes in the ready queue and the time quantum is q, then each process gets 1/n of the CPU time in chunks of at most q time units at once. No process waits more than (n-1)q time units.
Timer interrupts every quantum to schedule next process
Performance
  q large ⇒ FIFO
  q small ⇒ q must be large with respect to context switch, otherwise overhead is too high

<!-- slide 16 -->
### Example of RR with Time Quantum = 4
The Gantt chart is:
| Process | Arrival Time | Burst Time |
| P1 | 0 | 8 |
| P2 | 1 | 4 |
| P3 | 2 | 9 |
| P4 | 3 | 5 |
P1
P2
P3
P4
P1
P3
P4
P3
0
4
8
12
16
20
24
25
26
Ready Queue:
0
4
8
12
16
20
24
25
26
P1(8)
P3(9)
P4(5)
P1(4)
P4(5)
P1(4)
P1(4)
P3(5)
P3(5)
P4(1)
P4(1)
P3(1)
Empty
Empty
1
2
3
P2(4)
P2(4)
P3(9)
P2(4)
P3(9)
P4(5)

<!-- slide 17 -->
### Time Quantum and Context Switch Time
[image]

<!-- slide 18 -->
### Turnaround Time Varies With The Time Quantum
[image]
80% of CPU bursts should be shorter than quantum

<!-- slide 19 -->
### OPERATING SYSTEMS CPU Scheduling - Multilevel Queue, Multilevel Feedback Queue

<!-- slide 20 -->
### Multilevel Queue
Another class of scheduling algorithm needs- in which processes are classified into different groups, e.g.:
  foreground (interactive) processes
  background (batch) processes
They have different response time requirements-so different scheduling needs.
  Foreground processes may have priority over background processes.
  A multilevel queue-scheduling algorithm partitions the ready queue into several separate queues-we can see it in the figure of next slide:-
Each queue has its own scheduling algorithm:
  Foreground queue scheduled by – RR algorithm
  Background queue scheduled by – FCFS algorithm
Scheduling must be done between the queues:
  Fixed priority preemptive scheduling; (i.e., serve all from foreground then from background). Possibility of starvation.
  Time slice – each queue gets a certain amount of CPU time which it can schedule amongst its processes; i.e., foreground queue can be given 80% of the CPU time for RR-scheduling among its processes, while 20% to background in FCFS manner.

<!-- slide 21 -->
### Multilevel Queue Scheduling
[image: 5]
Notes: Partitions ready queue into several queues.

<!-- slide 22 -->
### Multilevel Feedback Queue scheduling
Multilevel Feedback Queue scheduling, allows a process to move between queues.
If a process uses too much CPU time, it will be moved to a lower priority queue.
Similarly, a process that waits too long in a lower-priority queue may me moved to a higher-priority queue.
Multilevel-feedback-queue scheduler defined by the following parameters:
  number of queues
  scheduling algorithms for each queue
  method used to determine when to upgrade a process
  method used to determine when to demote a process
  method used to determine which queue a process will enter when that process needs service

<!-- slide 23 -->
### Example of Multilevel Feedback Queue
[image: 5]
Three queues: (can see the figure in next slide)
  Q0 – RR with time quantum 8 milliseconds
  Q1 – RR time quantum 16 milliseconds
  Q2 – FCFS
Scheduling
  A new job enters queue Q0 which is served for RR
    When it gains CPU, job receives 8 milliseconds
    If it does not finish in 8 milliseconds, job is moved to queue Q1
  At Q1 job is again served RR and receives 16 additional milliseconds
    If it still does not complete, it is preempted and moved to queue Q2

<!-- slide 24 -->
### Example of Multilevel Feedback Queue
| Processes | Burst Time |
| P1 | 12 |
| P2 | 8 |
| P3 | 18 |
Queue 0 (Priority 0): Round-robin (quantum=2)
P1 (12)
P2 (8)
P3 (18)
P2 (8)
P3 (18)
P3 (18)
Queue 1 (Priority 1): Round-robin (quantum=4)
P1 (10)
P1 (10)
P2 (6)
P2 (6)
P3 (16)
Queue 2 (Priority 2): Round-robin (quantum=8)
P3 (16)
P1 (6)
P1 (6)
P2 (2)
P2 (2)
P3 (12)
P3 (12)
P3 (4)
Gantt Chart:
CPU:
0:
2:
4:
0:
Empty
0:
Empty
2:
4:
6:
10:
10:
14:
18:
24:
34:
Empty
4:
Empty
14:
Empty
38:
0
2
4
14
10
6
24
18
38
26
34
P1
P2
P3
P1
P2
P3
P1
P2
P3
P3
Problem 1:

<!-- slide 25 -->
### Example of Multilevel Feedback Queue
| Processes | Burst Time |
| P1 | 12 |
| P2 | 8 |
| P3 | 18 |
Gantt Chart:
CPU:
Problem 1:
0
2
4
14
10
6
24
18
38
26
34
P1
P2
P3
P1
P2
P3
P1
P2
P3
P3
Waiting Time:
P1=(0-0)+(6-2)+(18-10)=12
P2=(2-0)+(10-4)+(24-14)=18
P3=(4-0)+(14-6)+(26-18)+(34-34)=20
Avg=(12+18+20)/3=16.67
Response Time:
P1=(0-0)=0
P2=(2-0)=2
P3=(4-0)=4
Avg=(0+2+4)/3=2
Turnaround Time:
P1=(24-0)=24
P2=(26-0)=26
P3=(38-0)=38
Avg=(24+26+38)/3=29.33

<!-- slide 26 -->
### Example of Multilevel Feedback Queue
Gantt Chart:
CPU:
| Processes | Burst Time | Arrival Time | I/O Time |
| P1 | 12 | 0 | 6 / [2s I/O operation after total 4s of CPU allocation & 4s I/O operation after total 9s of CPU allocation] |
| P2 | 8 | 1 | 1 / [1s I/O operation after total 5s of CPU allocation] |
| P3 | 18 | 4 | N/A |
| P4 | 7 | 20 | 5 / [5s I/O operation after total 4s of CPU allocation] |
Queue 0 (Priority 0): Round-robin (quantum=2)
Queue 1 (Priority 1): Round-robin (quantum=4)
Queue 2 (Priority 2): Round-robin (quantum=8)
I/O Queue
0:
0:
Empty
0:
Empty
0:
Empty
P1 (12)
1:
2:
4:
11:
4:
10:
12:
P2 (8)
Empty
P3 (18)
Empty
P1 (8)
Promoted 1 level after I/O Operation
Empty
P2 (3)
Promoted 1 level after I/O Operation
13:
20:
22:
26:
38:
27:
32:
Empty
P4 (7)
Empty
P1 (3)
Promoted 1 level after I/O Operation
Empty
P4 (3)
Promoted 1 level after I/O Operation
Empty
2:
4:
6:
15:
8:
13:
15:
P1 (10)
P1 (10)
P2 (6)
P2 (6)
P3 (16)
P3 (16)
P3 (16)
P1 (6)
P3 (16)
P1 (6)
P2 (1)
P1 (6)
P2 (1)
19:
24:
25:
40:
29:
29:
40:
P2 (1)
P4 (5)
Empty
P1 (1)
Empty
P4 (1)
Empty
19:
30:
38:
41:
P3 (12)
Empty
P3 (4)
Empty
8:
8:
11:
11:
P1 (I/O: 2)
Empty
P2 (I/O: 1)
Empty
22:
22:
27:
27:
P1 (I/O: 4)
Empty
P4 (I/O: 5)
Empty
I/O Operations Gantt Chart
P1
P2
IDLE
IDLE
IDLE
P1
P4
8
10
11
12
22
26
27
32
P1
P3
P2
P1
P1
P2
P2
0
2
4
6
8
11
13
15
P3
P4
P1
P2
P1
P4
P1
19
22
24
25
27
29
30
P3
P4
P4
P3
38
40
41
45
I/O
I/O
I/O
I/O
Problem 2:

<!-- slide 27 -->
### Example of Multilevel Feedback Queue
Gantt Chart:
CPU:
| Processes | Burst Time | Arrival Time | I/O Time |
| P1 | 12 | 0 | 6 / [2s I/O operation after total 4s of CPU allocation & 4s I/O operation after total 9s of CPU allocation] |
| P2 | 8 | 1 | 1 / [1s I/O operation after total 5s of CPU allocation] |
| P3 | 18 | 4 | N/A |
| P4 | 7 | 20 | 5 / [5s I/O operation after total 4s of CPU allocation] |
I/O Operations Gantt Chart
P1
P2
IDLE
IDLE
IDLE
P1
P4
8
10
11
12
22
26
27
32
P1
P3
P2
P1
P1
P2
P2
0
2
4
6
8
11
13
15
P3
P4
P1
P2
P1
P4
P1
19
22
24
25
27
29
30
P3
P4
P4
P3
38
40
41
45
I/O
I/O
I/O
I/O
Problem 2:
Waiting Time:
P1=(0-0)+(6-2)+(11-10)+(19-13)+(27-26)+(29-29)
=12
P2=(2-1)+(8-4)+(13-12)+(24-15)=15
P3=(4-4)+(15-6)+(30-19)+(41-38)=23
P4=(22-20)+(25-24)+(38-32)+(40-40)=9
Avg=(12+15+23+9)/4=14.75
Response Time:
P1=(0-0)=0
P2=(2-1)=1
P3=(4-4)=0
P4=(22-20)=2
Avg=(0+1+0+2)/4=0.75
Turnaround Time:
P1=(30-0)=30
P2=(25-1)=24
P3=(45-4)=41
P4=(41-20)=21
Avg=(30+24+41+21)/4=29
