---
source: CSE321_pyq_quiz2_su22_ntr.pdf (the original, included in this repo)
fidelity: transcription (this file's own note says it is transcribed with confidence); page markers not recorded
verified: numbers checked against the PDF text layer on 2026-09-20
source-mtime: 2025-01-31T20:04:54
generated: earlier (2026-09); header added 2026-09-20
---
# CSE 321 Quiz 2 Summer 2022 (NTR)

Quiz 2 question paper for CSE321 Summer 2022 (Instructor: NTR) covering the convoy effect, preemptive priority scheduling, Round Robin tradeoffs, MLFQ starvation, and Round Robin Gantt chart calculation.
Docx last modified: 31-01-2025.

## Content

**Course**: CSE 321 – Operating Systems  
**Assessment**: QUIZ 2  
**Semester**: Summer Semester – 2022  
**Time**: 35 minutes | **Marks**: 20 points  

---

### Question 1 [CO3] (Total: 9 Points)
- **a)** [1 Point] What do you understand by the term “convoy effect”?
- **b)** [4 + 2 + 2 Points] Consider the following set of processes with the length of the CPU-burst time given in milliseconds. Draw the Gantt Charts illustrating the execution of these processes using **preemptive priority** (a biggest number implies a higher priority). Find average waiting time and average turnaround time for below data set.

| Process | Arrival Time | Priority | Burst Time |
| :--- | :--- | :--- | :--- |
| P1 | 0 | 2 | 4 |
| P2 | 1 | 4 | 2 |
| P3 | 2 | 6 | 3 |
| P4 | 3 | 10 | 5 |
| P5 | 4 | 8 | 1 |
| P6 | 5 | 12 | 4 |
| P7 | 6 | 9 | 6 |

---

### Question 2 [CO3] (Total: 4 Points)
- **a)** [2 Points] Write one possible advantage and disadvantage of the Round Robin (RR) scheduling algorithm.
- **b)** [2 Points] “Multilevel feedback queue prevents starvation”? Justify your answer.

---

### Question 3 [CO3] (Total: 7 Points)
Draw a Gantt chart and illustrating the execution of the process using **Round Robin** scheduling algorithm (time quantum = 8 s). Calculate average waiting time, and number of context switches.

| Processes | Arrival Time | Priority | Burst Time |
| :--- | :--- | :--- | :--- |
| P1 | 18 | 1 | 40 |
| P2 | 29 | 1 | 17 |
| P3 | 0 | 1 | 28 |
| P4 | 21 | 1 | 37 |
| P5 | 12 | 1 | 31 |\n