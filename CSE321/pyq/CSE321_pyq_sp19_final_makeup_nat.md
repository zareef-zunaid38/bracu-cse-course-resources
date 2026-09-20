---
source: CSE321_pyq_sp19_final_makeup_nat.doc (the original, not included in this repo)
fidelity: the summary above the divider is paraphrased and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 2 embedded image(s); see the summary or the source)
source-mtime: 2025-01-31T20:04:48
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE 321 Make-up Final Exam Spring 2019 (NAT)

Make-up final examination question paper for CSE321 Spring 2019 (Instructor: NAT) covering OS architecture, threads, Banker's Algorithm, CPU scheduling, paging, page replacement (LRU/Optimal), and memory placement algorithms.
Docx last modified: 31-01-2025.

## Content

**Department of CSE, BRAC University**  
**Course**: CSE321: Operating Systems  
**Exam**: Make-up Final Exam, Spring 2019  
**Time**: 2 hours | **Full Marks**: 60  
**Instructions**: Answer any Two from Section - A (2 X 10 = 20 Marks). Answer Section - B (20 Marks).

---

### SECTION - A (Answer any Two: 2 X 10 = 20 Marks)

#### Question 1
- **a)** [4 Marks] What OS’s Do? A modern computer system has a number of device controllers to do specific task; what is that task?
- **b)** [2 Marks] In multithreaded programs how can you cancel a thread and maintain a thread Pool.
- **c)** [4 Marks] What are differences between:
  1. Process and Thread (with figure). [2 Marks]
  2. Asymmetric and symmetric multiprocessing system. [2 Marks]

#### Question 2 [10 Marks]
Suppose, we have the following scenario in an OS. There are four processes and four resource types. Answer the following questions using Banker’s Algorithm:

| Processes | Max (A B C D) | Allocation (A B C D) | Available (A B C D) |
| :--- | :--- | :--- | :--- |
| **Pa** | 7 0 1 3 | 7 0 0 2 | **1 4 4 4** |
| **Pb** | 2 7 5 0 | 2 1 0 0 | |
| **Pc** | 2 12 5 6 | 0 6 3 3 | |
| **Pd** | 1 6 5 6 | 0 2 1 2 | |

- **a)** Calculate Need matrix.
- **b)** Is this system in safe state? If yes, then find the safe sequence or if not, then provide necessary explanation?
- **c)** What happens if process Pb request at this moment for $(0, 4, 2, 0)$? Whether Banker’s algorithm grants the request or not? If grants the request find the safe sequence.

#### Question 3 [10 Marks]
- **a)** [5 Marks] What aging is? How multilevel feedback queue work? What are the optimization criteria of process scheduling?
- **b)** [5 Marks] What are the difference between Internal and External Fragmentation? Name three requirements for solving critical section problem.

#### Question 4 [10 Marks]
- **a)** [2 Marks] How to predict the next CPU burst time of the process for SJF scheduling algorithm?
- **b)** [8 Marks] Considering following set of processes, construct Gantt Chart for:
  1. SJF (preemptive)
  2. RR (time quantum = 3 milliseconds)
  3. Priority scheduling (a smaller priority number implies a higher priority) CPU scheduling algorithm.  
  *(*** Note: Need to consider Arrival time).*

| Process | Burst Time | Arrival Time | Priority |
| :--- | :--- | :--- | :--- |
| P1 | 10 | 0 | 1 |
| P2 | 13 | 13 | 2 |
| P3 | 9 | 15 | 3 |
| P4 | 8 | 20 | 5 |
| P5 | 5 | 50 | 7 |
| P6 | 12 | 90 | 9 |

---

### SECTION – B (20 Marks)

#### Question 6 [20 Marks]
- **a)** [6 Marks] Assume that page size = 4 bytes and Physical Memory = 32 bytes. If CPU generates logical addresses 8, 3, 13, 0, 17 and 7 respectively then how the users’ view of memory can be mapped into physical memory?
  - Figure (transcribed): Logical Memory holds P0 = `I`, P1 = `Love`, P2 = `CSE`, P3 = `And`, P4 = `BRAC`, P5 = `University`. PMT (page table): P0 → 7, P1 → 9, P2 → 4, P3 → 8, P4 → 5, P5 → 6. Main Memory is an empty box to be filled in.
- **b & c)** [6 Marks] A very simple computer has 4 page frames. And a process makes the following list of page references:  
  `1, 2, 3, 4, 1, 5, 2, 3, 1, 2, 3, 4, 5, 1, 2, 5, 4, 3, 3, 1, 0, 4, 3, 1, 2`  
  How many page fault occur using **LRU** and **Optimal** page replacement algorithms?
- **d)** [3 Marks] When does page fault occur? Draw a figure to illustrate how Shared Pages (sharing of common code) work in paged memory management system.
- **e)** [5 Marks] Given memory partitions of **210k, 350k, 250k, 190k, 250k and 452k** (in order, top to bottom), apply **first fit** and **best fit** algorithms to place processes with the space requirement of **250k, 425k, 212k, 160k, 210k, and 440k** (in order). Which algorithm makes the most effective use of memory?\n

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, via a LibreOffice text conversion of the legacy .doc; formatting and images are not reproduced -->

﻿Department of CSE, BRAC University
CSE321:Operating Systems
                                 Make-up Final Exam, Spring 2019
Time: 2hours				                     		          Full Marks: 60
Answer any Two from Section - A (2 X 10 = 20)

1.
Section - A
a. What OS’s Do?  A modern computer system has a number of device controllers to do specific task; what is that task?
b. In multithreaded programs how can you cancel a thread and maintain a thread Pool.
c. What are differences between:
  i. Process and Thread (with figure).
  ii. Asymmetric and symmetric multiprocessing system.

[4]

[2]

[2]
[2]
2.
Suppose, we have the following scenario in an OS. There are four processes and four resource types. Answer the following questions using Banker’s Algorithm.	


    a. Calculate Need matrix.
    b. Is this system in safe state? If yes, then find the safe sequence or if not, then provide necessary explaination?
    c. What happens if process Pb request at this moment for (0, 4, 2, 0)? Whether Bnker’s algorithm grants the request or not? If grants the request find the safe sequence. 	
[10]
3.
a. What aging is? How multilevel feedback queue work? What are the optimization criteria of process scheduling?
b. What are the difference between Internal and External Fragmentation?  Name three requirements for solving critical section problem.
[5]

[5]
4.
a. How to predict the next CPU burst time of the process for SJF scheduling algorithm? 
b. Considering following set of processes, construct Gantt Chart for SJF (preemptive) and RR ( time quantum = 3 milliseconds) and Priority scheduling (a smaller priority number implies a higher priority) CPU scheduling algorithm.
*** Note: Need to consider Arrival time.
Process
Burst Time
Arrival Time
Priority
P1
10
0
1
P2
13
13
2
P3
9
15
3
P4
8
20
5
P5
5
50
7
P6
12
90
9

[2]
[8]

SECTION – B(20 marks)


6.

    a. Assume that page size = 4 bytes and Physical Memory = 32 bytes. If CPU generates logical addresses 8, 3, 13, 0, 17 and 7 respectively then how the users’ view of memory can be mapped into physical memory? 

    b. a. A very simple computer has 4 page frames. And a process makes the following list of page references: 1, 2, 3, 4, 1, 5, 2, 3, 1, 2, 3, 4, 5, 1, 2, 5, 4, 3, 3, 1, 0, 4, 3, 1, 2.
    c. How many page fault occur using LRU and Optimal page replacement algorithms?
    d. When does page fault occur? Draw a figure to illustrate how Shared Pages (sharing of common code) work in paged memory management system.
    e. a. Given memory partitions of 210k, 350k, 250k,190k, 250k and 452k (in order, top to bottom), apply first fit and best fit algorithms to place processes with the space requirement of 250k, 425k, 212k, 160, 210, and 440k (in order). Which algorithm makes the most effective use of memory?



[6]







[6]
[3]

[5]
										
------THE END-----
