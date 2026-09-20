---
source: CSE321_pyq_su19_final_nat.docx (the original, not included in this repo)
fidelity: the summary above the divider is paraphrased and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 3 embedded image(s); see the summary or the source)
source-mtime: 2025-01-31T20:04:52
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Final Examination Summer 2019

CSE321 Operating Systems Summer 2019 Final exam covering logical to physical memory mapping, dynamic memory partitioning (worst-fit/best-fit), paging benefits and page fault handling steps, page replacement algorithms (OPT vs LRU), demand paging, precedence graph process synchronization using P() and V() semaphores, Resource Allocation Graph deadlock detection, Banker's algorithm, SJF CPU scheduling, process management activities, virtual machines, process state transitions, and thread cancellation/creation.
Docx last modified: 31-01-2025.

## Content

### Exam Metadata
- **Institution**: BRAC University, Department of Computer Science and Engineering
- **Course**: CSE 321: Operating Systems
- **Examination**: Semester Final
- **Semester**: Summer 2019
- **Duration**: 2 Hours 30 minutes
- **Full Marks**: 64

---

### Section A [CO6] [Answer any Two (2)]

#### Question 1 [Marks: 4 + 4 = 8]
- **a)** Assume that page size = 3 KB and Physical Memory = 36 KB. If CPU generates logical addresses 5, 9, 2, and 16 respectively, then develop the users' view of memory which is mapped into physical memory (given Logical Address Space: P0=CSE101, P1=CSE220, P2=CSE110, P3=CSE330, P4=CSE420, P5=CSE321; and Page Map Table PMT: 0→6, 1→2, 2→9, 3→4, 4→11, 5→7). [4]
- **b)** In a particular time the snapshot of Main memory given below for dynamic partition:
  - Free holes in memory: `22k`, `12k`, `31k`, `5k`, `46k` (with occupied gray blocks between them).
  - Apply worst fit and best fit algorithms to place processes with the space requirement of 26k, 30k, 15k, 20k, and 6k. Explain which algorithm makes the most effective use of memory? [4]

#### Question 2 [Marks: 6 + 2 = 8]
- **a)** Explain in which terms Paging is more beneficial than Contiguous allocation. Illustrate page fault steps and describe briefly. [2+4]
- **b)** List disadvantages of Static and Dynamic memory allocation. [2]

#### Question 3 [Marks: 6 + 2 = 8]
- **a)** CPU generates page number sequences as follows: `3, 0, 0, 1, 1, 7, 0, 3, 0, 4, 4, 3, 4, 2, 1, 2, 0, 4, 3, 1, 7, 0, 4, 3, 1, 1, 7, 0` and there are only 5 frames to accommodate. Calculate the number of page faults for optimal and LRU Page replacement algorithm. Also find the efficiency for both page replacement algorithms. [6]
- **b)** Define demand paging. [2]

---

### Section B [Answer All Questions]

#### Question 4 [CO5] [Marks: 4 + 4 = 8]
- **a)** Solve the precedence graph of process synchronization drawn below using semaphore variables (Node statements running independently: $S_1 \to S_2, S_3, S_4, S_5$; $S_2 \to S_6$; $S_3 \to S_6, S_7$; $S_4 \to S_7$; $S_5 \to S_8$; $S_6 \to S_8$; $S_7 \to S_8$). Write code using $P()$ and $V()$ operations only. [4]
- **b)** Suppose, in our office, we have a set of resource types, $R = \{R1, R2, R3, R4\}$ and a set of processes, $P = \{P1, P2, P3, P4, P5\}$. All the resource types in $R$ have 3, 1, 4, and 2 instances respectively. Processes are organized such that: [4]
  - P1 is holding 2 instances of R1
  - P2 is holding 1 instance of R3
  - P3 is holding 1 instance of R4
  - P5 requests 2 instances of R3
  - P4 requests 1 instance of R4
  - P3 requests 1 instance of R2
  - P2 requests 1 instance of R1
  - P2 is holding 1 instance of R2
  - P1 is requesting 1 instance of R4
  - P3 is holding 1 instance of R3
  
  Construct a resource allocation graph for the above scenario and identify whether there is deadlock or not.

#### Question 5 [CO5] [Marks: 2 + 3 + 3 = 8]
Suppose, we have the following scenario in an OS with five processes and four resource types:

| Processes | Max (A B C D) | Allocation (A B C D) | Available (A B C D) |
| :--- | :--- | :--- | :--- |
| **P1** | 4 2 3 3 | 2 2 1 0 | 1 1 2 1 |
| **P2** | 3 4 2 0 | 1 2 0 0 | |
| **P3** | 1 4 3 1 | 1 2 2 0 | |
| **P4** | 3 3 0 2 | 1 1 0 2 | |
| **P5** | 2 1 4 2 | 1 1 3 2 | |

- Calculate Need matrix. [2]
- Is this system in safe state? If yes, then show the safe sequence; if no, provide necessary explanation. [3]
- If a request $(0, 1, 1, 0)$ from process P3 arrives for additional resources, analyze whether Banker's algorithm can grant the request or not. [3]

#### Question 6 [CO3] [Marks: 1 + 4 + 3 = 8]
- **a)** List the optimization criteria of CPU scheduling. [1]
- **b)** The Shortest Job First (SJF) preemptive CPU scheduling technique (SRTF) preempts an executing process. Using the SJF preemptive policy, construct a Gantt Chart and compute the waiting time for the 6 processes tabulated below (time in ms): [4+3]

  | Process | Burst Time | Arrival Time |
  | :--- | :--- | :--- |
  | **P1** | 8 | 3 |
  | **P2** | 12 | 10 |
  | **P3** | 8 | 13 |
  | **P4** | 6 | 14 |
  | **P5** | 7 | 19 |
  | **P6** | 8 | 88 |

#### Question 7 [CO1] [Marks: 4 + 4 = 8]
- **a)** Write five major activities of an operating system in regard to process management. [4]
- **b)** What is the main advantage for an operating system designer of using virtual machine architecture? How does the guest operating system function on the host operating system? [Hint: VMWare]. [4]

#### Question 8 [CO2] [Marks: 4 + 4 = 8]
- **a)** Can a process make a transition from the running to the ready and waiting state? Explain why or why not? [4]
- **b)** Define IPC? Discuss two models of IPC. [4]

#### Question 9 [CO4] [Marks: 3 + 5 = 8]
- **a)** In a multithreading program state write how a thread can be canceled. [3]
- **b)** Define the resources that are used in thread creation and how do they differentiate from those resources used when a process is created? [5]

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets, headers/footers, footnotes, comments; list numbering, formatting and images are not reproduced -->

BRAC UNIVERSITY

Department of Computer Science and Engineering

| Examination: Semester Final / Duration: 2 Hours 30 minutes | Semester :Summer 2019 / Full Marks:64 |

CSE 321: Operating Systems

Answer the following questions.

Figures in the right margin indicate marks.

| Name: | ID: | Section: |

Section-A [CO6] [Answer any Two (2)]

| 1. | a) | Assume that page size = 3KB and Physical Memory = 36KB. If CPU generates logical addresses 5, 9, 2 and 16 respectively then developed the users’ view of memory which is mapped into physical memory? | 4 |

|  | b) | In a particular time the snapshot of Main memory given below for dynamic partition. Apply worst fit and best fit algorithms to place processes with the space requirement of 26k, 30k, 15k, 20k, and 6k. Explain which algorithm makes the most effective use of memory? | 4 |

| 2. | a) | Explain in which term Paging is more beneficial than Contiguous allocation. Illustrate page fault steps and describe briefly. | 2+4 |

|  | b) | List disadvantage of Static and Dynamic memory allocation. | 2 |

| 3. | a) | CPU generates page no sequences are as follows: 3, 0, 0, 1, 1, 7, 0, 3, 0, 4, 4, 3, 4, 2, 1, 2, 0, 4, 3, 1, 7, 0, 4, 3, 1, 1, 7, 0 and there are only 5 frames to accommodate. Calculate he number of page fault for optimal and LRU Page replacement algorithm. Also find the efficiency for both of the page replacement algorithm. | 6 |

|  | b) | Define demand paging. | 2 |

| Section-B [Answer All Questions] |

| 4. CO5 | a) | Solve the graph of process synchronization drawn below using semaphore variables. [Note: Each node represents a statement (S) which is running independently. Write code using P() and V() operations only] | 4 |

|  | b) | Suppose, in our office, we have a set of resource types, R = {R1, R2, R3, R4} and a set of processes, P = {P1, P2, P3, P4, P5}. All the resource types in R have 3, 1, 4, and 2 instances respectively. Processes are organized such that P1 is holding 2 instances of R1, P2 is holding 1 instance of R3, P3 is holding 1 instance of R4, P5 requests 2 instances of R3, P4 requests 1 instance of R4, P3 requests 1 instance of R2, P2 requests 1 instance of R1, P2 is holding 1 instance of R2, P1 is requesting 1 instance of R4, P3 is holding 1 instances of R3.Construct a resource allocation graph for the above scenario and identify weather there is deadlock or not. | 4 |

| 5. CO5 |  | Suppose, we have the following scenario in an OS. There are five processes and four resource types. Answer the following questions using Banker’s Algorithm. / | Processes | Max | Allocation | Available | / |  | A | B | C | D | A | B | C | D | A | B | C | D | / | P1 | 4 | 2 | 3 | 3 | 2 | 2 | 1 | 0 | 1 | 1 | 2 | 1 | / | P2 | 3 | 4 | 2 | 0 | 1 | 2 | 0 | 0 |  |  |  |  | / | P3 | 1 | 4 | 3 | 1 | 1 | 2 | 2 | 0 |  |  |  |  | / | P4 | 3 | 3 | 0 | 2 | 1 | 1 | 0 | 2 |  |  |  |  | / | P5 | 2 | 1 | 4 | 2 | 1 | 1 | 3 | 2 |  |  |  |  | / Calculate Need matrix / Is this system in safe state? If yes, then show the safe sequence or if no, then provide necessary explaination? / If a request (0,1,1,0) from process P3 arrives for additional resources, analyze whether Banker’s algorithm can grant the request or not. | 2+3+3 |

| 6. CO3 |  | List the optimization criteria of CPU scheduling? / The SJF CPU scheduling technique preempts an executing process. Using the SJF policy, construct a Gantt Chart and compute the waiting time for the 6 processes tabulated below (time in milliseconds): / | Process | Burst Time | Arrival Time | / | P1 | 8 | 3 | / | P2 | 12 | 10 | / | P3 | 8 | 13 | / | P4 | 6 | 14 | / | P5 | 7 | 19 | / | P6 | 8 | 88 | | 1+4+3 |

| 7. CO1 | a) | Write five major activities of an operating system in regard to process management. | 4+4 |

|  | b) | Write what is the main advantage for an operating system designer of using virtual machine architecture? How does the guest operating system function on the host operating system? [Hint: VMWare]. |  |

| 8. CO2 | a) | Can a process make a transition from the running to the ready and waiting state? Explain why or why not? | 4+4 |

|  | b) | Define IPC? Discuss two models of IPC. |  |

| 9. / CO4 | a) | In a multithreading program state write how a thread can be canceled. | 3+5 |

|  | b) | Define the resources that are used in thread creation and how do they differentiate from those resources used when a process is created? |  |

***END***
