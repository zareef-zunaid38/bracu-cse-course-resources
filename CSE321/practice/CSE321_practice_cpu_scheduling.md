---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# Practice Problems on CPU Scheduling

CSE321 practice problems and numerical solutions for CPU scheduling algorithms: FCFS, SRJF, Preemptive Priority, Round Robin, and Multilevel Feedback Queue (MLFQ).
Docx last modified: 06-10-2025.

## Content

### Problem 1: Standard Scheduling Algorithm Comparison
Given arrival times, burst times, and priority values (lower number = higher priority) for the processes below. Draw Gantt charts and calculate average waiting time and turnaround time using:
1. First Come First Serve (FCFS)
2. Shortest Remaining Job First (SRJF)
3. Preemptive Priority
4. Round Robin (RR, time quantum: 5)

| Process | Arrival Time | Burst Time | Priority |
| :--- | :--- | :--- | :--- |
| P1 | 0 | 8 | 4 |
| P2 | 2 | 6 | 1 |
| P3 | 2 | 1 | 2 |
| P4 | 1 | 9 | 2 |
| P5 | 3 | 3 | 3 |

#### Answers:
- **I. FCFS**:
  - Avg. Waiting Time: `12.8`
  - Avg. Turnaround Time: `18.2`
- **II. SRJF**:
  - Avg. Waiting Time: `6.2`
  - Avg. Turnaround Time: `11.6`
- **III. Preemptive Priority**:
  - Avg. Waiting Time: `9.2`
  - Avg. Turnaround Time: `14.6`
- **IV. Round Robin (quantum: 5)**:
  - Avg. Waiting Time: `15.0`
  - Avg. Turnaround Time: `20.4`

---

### Problem 2: Multilevel Feedback Queue (MLFQ) without I/O
Given arrival times and burst times below, draw a Gantt chart using the Multilevel Feedback Queue scheduling algorithm showing the states of the ready queues of different levels. Calculate average waiting time, response time, and turnaround time.
- **Queue Configuration**: 3 queues with decreasing priority, all using Round Robin internally:
  - **Q1**: Priority 0 (highest), Time Quantum = 3
  - **Q2**: Priority 1, Time Quantum = 5
  - **Q3**: Priority 2 (lowest), Time Quantum = 10

| Process | Arrival Time | Burst Time |
| :--- | :--- | :--- |
| P1 | 0 | 10 |
| P2 | 2 | 23 |
| P3 | 5 | 18 |
| P4 | 7 | 16 |

#### Answers:
- Avg. Waiting Time: `34`
- Avg. Response Time: `1`
- Avg. Turnaround Time: `50.75`

---

### Problem 3: Multilevel Feedback Queue (MLFQ) with I/O Promotion
Given arrival times, burst times, and I/O requests below, draw a Gantt chart using the Multilevel Feedback Queue scheduling algorithm showing the states of ready queues at different levels. Calculate average waiting time, response time, and turnaround time.
- **Queue Configuration**: 3 queues with decreasing priority (Round Robin):
  - **Q1**: Priority 0, Quantum = 3
  - **Q2**: Priority 1, Quantum = 5
  - **Q3**: Priority 2, Quantum = 10
- **I/O Rule**: In case of an I/O request, a process will be promoted one level upon I/O completion.

| Process | Arrival Time | Burst Time | I/O Request |
| :--- | :--- | :--- | :--- |
| P1 | 0 | 10 | 3s I/O after 7s CPU |
| P2 | 2 | 23 | 4s I/O after 5s CPU; 3s I/O after 12s CPU |
| P3 | 5 | 18 | N/A |
| P4 | 7 | 16 | 2s I/O after 11s CPU |

#### Answers:
- Avg. Waiting Time: `27.25`
- Avg. Response Time: `1`
- Avg. Turnaround Time: `47.25`\n