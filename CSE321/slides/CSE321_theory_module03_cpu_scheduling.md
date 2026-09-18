# CSE321 Module 03: CPU Scheduling — sidecar (read this, not the .pptx)

Lecture slides covering CPU scheduling concepts, CPU-I/O burst cycles, preemptive vs non-preemptive scheduling, dispatcher, scheduling criteria, and scheduling algorithms (FCFS, SJF, SRTF, Priority, Round Robin, Multilevel Queue).
Pptx last modified: 12-07-2025.

## Content

### Slides 1–4: Basic Concepts & CPU-I/O Burst Cycle
- **Maximum CPU utilization** is obtained with multiprogramming.
- Process execution consists of a cycle of **CPU execution** and **I/O wait**:
  - Starts with a CPU burst, followed by an I/O burst, then another CPU burst, terminating on a CPU burst.
  - Burst durations typically have an exponential or hyperexponential frequency curve (large number of short bursts, small number of long bursts).
- **CPU Scheduler:** Selects from among the processes in memory that are ready to execute and allocates the CPU to one of them.

### Slides 5–6: Preemptive vs Non-Preemptive Scheduling
CPU scheduling decisions take place under four circumstances:
1. When a process switches from running to waiting state (e.g., I/O request, `wait()` invocation).
2. When a process switches from running to ready state (e.g., timer interrupt).
3. When a process switches from waiting to ready state (e.g., I/O completion).
4. When a process terminates.
- **Non-preemptive (Cooperative) Scheduling:** Scheduling takes place only under circumstances 1 and 4. Once CPU is allocated to a process, it keeps the CPU until it releases it either by terminating or switching to waiting state.
- **Preemptive Scheduling:** Scheduling occurs under all circumstances (1, 2, 3, 4). OS can interrupt a running process when a higher-priority process arrives or time quantum expires. Introduces overhead and race condition risks on shared kernel data.

### Slide 7: Dispatcher
- Module that gives control of the CPU to the process selected by the short-term scheduler.
- **Functions:**
  - Switching context.
  - Switching to user mode.
  - Jumping to the proper location in the user program to restart that program.
- **Dispatch Latency:** Time it takes for the dispatcher to stop one process and start another running.

### Slides 8–9: Scheduling Criteria
Metrics used to evaluate and compare CPU scheduling algorithms:
1. **CPU Utilization:** Percentage of time CPU is busy executing user/OS processes (typically 40% to 90%). $\rightarrow$ Maximize.
2. **Throughput:** Number of processes completed per unit time. $\rightarrow$ Maximize.
3. **Turnaround Time ($TAT$):** Interval from the time of submission of a process to the time of completion ($TAT = \text{Completion Time} - \text{Arrival Time}$). $\rightarrow$ Minimize.
4. **Waiting Time ($WT$):** Total amount of time a process spends waiting in the ready queue ($WT = TAT - \text{Burst Time}$). $\rightarrow$ Minimize.
5. **Response Time ($RT$):** Time from submission of a request until the first response is produced ($RT = \text{First CPU Allocation Time} - \text{Arrival Time}$). $\rightarrow$ Minimize (crucial for interactive systems).

### Slides 10–12: First-Come, First-Served (FCFS) Scheduling
- The process that requests the CPU first is allocated the CPU first (implemented via FIFO queue).
- Non-preemptive.
- Simple to understand and implement.
- **Disadvantage:** Average waiting time is often quite long and varies substantially depending on arrival order.
- **Convoy Effect:** All short processes wait for one long CPU-bound process to release the CPU, resulting in lower CPU and device utilization.

### Slides 13–16: Shortest-Job-First (SJF) Scheduling
- Associates with each process the length of its next CPU burst. When CPU is available, it is assigned to the process with the smallest next CPU burst.
- **Optimality:** SJF is optimal in that it gives the minimum average waiting time for a given set of processes.
- **Variants:**
  - *Non-preemptive SJF:* Once CPU allocated, process runs to completion of burst.
  - *Preemptive SJF / Shortest-Remaining-Time-First (SRTF):* If a new process arrives with a CPU burst shorter than remaining time of currently executing process, the current process is preempted.
- **Difficulty:** Knowing the length of the next CPU burst in advance. Often estimated using exponential smoothing:
  $$\tau_{n+1} = \alpha t_n + (1 - \alpha)\tau_n$$
  where $t_n$ is actual burst length, $\tau_n$ is predicted value, and $0 \le \alpha \le 1$.

### Slides 17–19: Priority Scheduling
- A priority number (integer) is associated with each process. CPU is allocated to the process with the highest priority (convention: smallest integer = highest priority).
- Can be preemptive or non-preemptive.
- (Note: SJF is a special case of priority scheduling where priority is the inverse of predicted burst time).
- **Major Problem:** **Starvation (Indefinite Blocking)** — low priority processes may never execute if higher priority processes keep arriving.
- **Solution:** **Aging** — gradually increasing the priority of processes that wait in the system for a long time.

### Slides 20–24: Round-Robin (RR) Scheduling
- Designed especially for time-sharing systems.
- A small unit of time, called a **time quantum (time slice)**, is defined (generally 10 to 100 milliseconds).
- Ready queue treated as a circular FIFO queue. CPU scheduler goes around ready queue, allocating CPU to each process for a time interval of up to 1 time quantum.
- Preemptive: If process burst exceeds quantum, it is interrupted and put at tail of ready queue.
- **Performance depends heavily on time quantum ($q$):**
  - If $q \to \infty$: RR behaves like FCFS.
  - If $q$ is very small: RR causes high context-switch overhead (*processor sharing*).
  - Rule of thumb: 80% of CPU bursts should be shorter than the time quantum $q$.

### Slides 25–27: Multilevel Queue & Multilevel Feedback Queue Scheduling
- **Multilevel Queue Scheduling:**
  - Ready queue partitioned into separate queues based on process properties (e.g., Foreground / Interactive vs Background / Batch).
  - Each queue has its own scheduling algorithm (e.g., RR for foreground, FCFS for background).
  - Scheduling between queues is usually fixed-priority preemptive scheduling or time-sliced across queues.
- **Multilevel Feedback Queue Scheduling:**
  - Allows a process to move between queues based on its CPU burst characteristics.
  - If a process uses too much CPU time, it is moved to a lower-priority queue (aging handles starvation by moving old processes to higher-priority queues).
  - Most general and complex CPU scheduling algorithm.
