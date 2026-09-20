---
source: CSE321_pyq_sp22_final.docx (the original, not included in this repo)
fidelity: the summary above the divider is paraphrased and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 1 embedded image(s); see the summary or the source)
source-mtime: 2025-01-31T20:04:52
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Final Examination Spring 2022

CSE321 Operating Systems Spring 2022 Final exam paper covering race conditions, semaphore synchronization, Banker's algorithm, Resource Allocation Graphs, dynamic memory allocation (worst-fit/best-fit), and paging address translation.
Docx last modified: 31-01-2025.

## Content

### Exam Metadata
- **Institution**: BRAC University, Department of Computer Science and Engineering
- **Course**: CSE 321: Operating Systems
- **Examination**: Final
- **Semester**: Spring 2022
- **Duration**: 2 Hours
- **Full Marks**: 40

---

### Question 1 [CO5] [Marks: 2+1 + 2 + 5 = 10]
- **a)** Explain Race conditions with an example. Mention how we protect the system from this phenomenon. [2+1]
- **b)** Explain with a code example how a careless ordering of semaphore operations can lead to a deadlock situation among two processes. [2]
- **c)** Suppose, you have to design an online consultation system for the teachers and students of your university. There are certain constraints that you have to keep in mind while designing the system: [5]
  1. The teachers can set their status whether they are available to give consultation or not. A teacher will set him unavailable after the `giveConsultation()` function.
  2. The students will enter a voice channel if the teacher is available for consultation.
  3. The students will wait if one student is in consultation with the teacher.
  
  Now, you have to design the teacher and student function using semaphores so that synchronization can be achieved among them maintaining the constraints mentioned above. You can use the following code template given below and complete it. Mention the initial semaphore values before writing the functions.
  ```c
  //initialize the semaphore values here
  teacher(){
      // write semaphore code here
      giveConsultation(); 
      // write semaphore code here
  }

  student(){
      // write semaphore code here
      takeConsultation(); 
      // write semaphore code here
  }
  ```

---

### Question 2 [CO5] [Marks: 2 + 2 + 3 + (3+2+3) = 15]
- **a)** Explain how Banker's algorithm can help to find the processes that are causing a deadlock in a system. [2]
- **b)** Describe some strategies for deadlock prevention that can break the hold-and-wait condition. [2]
- **c)** Suppose, in an office, we have a set of resource types, $R = \{R1, R2, R3\}$ and a set of processes, $P = \{P1, P2, P3, P4\}$. $R1, R2$, and $R3$ have 4, 2, and 2 instances respectively. [3]
  - i) P1 is holding 2 instances of R1
  - ii) P2 requests 1 instance of R3
  - iii) P3 requests 2 instances of R2
  - iv) P2 requests 1 instance of R1
  - v) P2 is holding 1 instance of R2
  - vi) P3 is holding 1 instance of R3
  
  Construct a resource allocation graph for the above scenario and identify whether there is a deadlock or not.

- **d)** Consider the following snapshot of a system:

  | Process | Allocation (A B C D) | Max (A B C D) | Available (A B C D) |
  | :--- | :--- | :--- | :--- |
  | **P1** | 0 0 1 2 | 0 0 2 3 | 1 2 2 0 |
  | **P2** | 1 0 0 0 | 1 2 2 0 | |
  | **P3** | 1 3 5 4 | 2 3 5 6 | |
  | **P4** | 0 0 0 1 | 2 2 0 1 | |

  - **i.** Is the system in a safe state? [3]
  - **ii.** Can P3's request $(1\ 0\ 0\ 0)$ be safely granted immediately? [2]
  - **iii.** If P3's request is granted immediately, does the system enter a deadlock? [3]

---

### Question 3 [CO6] [Marks: 3 + 2 + (4+2) + 4 = 15]
- **a)** Explain the disadvantage of using Contiguous allocation and how Paging is more beneficial than Contiguous allocation. [3]
- **b)** Explain how the operating system's behavior and hardware mechanism for logical to physical address translation ensure that one process cannot access the memory allocated for another process. [2]
- **c)** At a particular time, the snapshot of the Main memory is given below for dynamic partition. Gray portions of the memory are occupied space:
  - Free hole sizes in order: `22k`, `12k`, `31k`, `5k`, `46k`.
  - **i)** Apply worst-fit and best-fit algorithms to allocate processes with the space requirement of P1(26k), P2(30k), P3(15k), P4(20k), and P5(6k). [4]
  - **ii)** Explain which algorithm makes the most effective use of memory? [2]
- **d)** Suppose, in a system, there are two processes — P1 (16 bytes) and P2 (12 bytes) with a page size of 4 bytes. The main memory size of the system is 32 bytes. Page tables of both processes are given below: [4]

  **Page Table of P1:**
  | Page ($p$) | Frame ($f$) |
  | :--- | :--- |
  | 0 | 5 |
  | 1 | 0 |
  | 2 | 7 |
  | 3 | 2 |

  **Page Table of P2:**
  | Page ($p$) | Frame ($f$) |
  | :--- | :--- |
  | 0 | 6 |
  | 1 | 1 |
  | 2 | 4 |

  Find the corresponding physical address of the following logical addresses:
  - **i.** Address `1011` of P1
  - **ii.** Address `0100` of P1
  - **iii.** Address `0111` of P2
  - **iv.** Address `1010` of P2

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, headers/footers, footnotes, comments, link targets; formatting and images are not reproduced -->

BRAC UNIVERSITY

Department of Computer Science and Engineering

| Examination: Final / Duration: 2 Hours | Semester: Spring 2022 / Full Marks: 40 |

CSE 321: Operating Systems

Answer the following questions.

Figures in the right margin indicate marks.

| 1. / CO5 | Explain Race conditions with an example. Mention how we protect the system from this phenomenon. / Explain with a code example how a careless ordering of semaphore operations can lead to a deadlock situation among two processes. / Suppose, you have to design an online consultation system for the teachers and students of your university. There are certain constraints that you have to keep in mind while designing the system - / i. the teachers can set their status whether they are available to give consultation or not. A teacher will set him unavailable after the giveConsultation() function. / ii. the students will enter a voice channel if the teacher is available for consultation. / iii. the students will wait if one student is in consultation with the teacher / Now, you have to design the teacher and student function using semaphores so that synchronization can be achieved among them maintaining the constraints mentioned above. You can use the following code template given below and complete it. Mention the initial semaphore values before writing the functions. / | //initialize the semaphore values here / teacher(){ / // write semaphore code here / giveConsultation(); / // write semaphore code here / } / student(){ / // write semaphore code here / takeConsultation(); / // write semaphore code here / } | | [2+1] / [2] / [5] |

| 2. / CO5 | Explain how Banker's algorithm can help to find the processes that are causing a deadlock in a system. / Describe some strategies for deadlock prevention that can break the hold-and-wait condition. / Suppose, in an office, we have a set of resource types, R = {R1, R2, R3} and a set of processes, P = {P1, P2, P3, P4}. R1, R2, and R3 have 4, 2, and 2 instances respectively. / P1 is holding 2 instances of R1, / P2 requests 1 instance of R3 / P3 requests 2 instances of R2 / P2 requests 1 instance of R1 / P2 is holding 1 instance of R2 / P3 is holding 1 instance of R3 / Construct a resource allocation graph for the above scenario and identify whether there is a deadlock or not. / Consider the following snapshot of a system: / |  |  | Allocation |  | Max | / |  |  | A | B | C | D |  | A | B | C | D | / | P1 |  | 0 | 0 | 1 | 2 |  | 0 | 0 | 2 | 3 | / | P2 |  | 1 | 0 | 0 | 0 |  | 1 | 2 | 2 | 0 | / | P3 |  | 1 | 3 | 5 | 4 |  | 2 | 3 | 5 | 6 | / | P4 |  | 0 | 0 | 0 | 1 |  | 2 | 2 | 0 | 1 | / |  |  | Available |  |  |  |  |  | / |  |  | 1 | 2 | 2 | 0 |  |  |  |  |  | / i. Is the system in a safe state? / ii. Can P3’s request (1 0 0 0) be safely granted immediately? / iii. If P3’s request is granted immediately, does the system enter a deadlock? | [2] / [2] / [3] / [3] / [2] / [3] |

| 3. / CO6 | Explain the disadvantage of using Contiguous allocation and how Paging is more beneficial than Contiguous allocation. / Explain how the operating system’s behavior and hardware mechanism for logical to physical address translation ensure that one process cannot access the memory allocated for another process. / At a particular time, the snapshot of the Main memory is given below for dynamic partition. Gray portions of the memory are occupied space. /   (text box) 22k /   (text box) 12k /   (text box) 31k /   (text box) 5k /   (text box) 46k / Apply worst-fit and best-fit algorithms to allocate processes with the space requirement of P1(26k), P2(30k), P3(15k), P4(20k), and P5(6k). / Explain which algorithm makes the most effective use of memory? / Suppose, in a system, there are two processes - P1 (16 bytes) and P2 (12 bytes) with a page size of 4 bytes. The main memory size of the system is 32 bytes. Page tables of both processes are given below. /   (text box) 5 /   (text box) 0 /   (text box) 7 /   (text box) 2 /   (text box) 0 /   (text box) 1 /   (text box) 2 /   (text box) 3 /   (text box) f /   (text box) p /   (text box) 6 /   (text box) 1 /   (text box) 4 /   (text box) 0 /   (text box) 1 /   (text box) 2 /   (text box) f /   (text box) p /   (text box) Page Table /   (text box) of P1 /   (text box) Page Table /   (text box) of P2 / Find the corresponding physical address of the following logical addresses - / i. address 1011 of P1 / ii. address 0100 of P1 / iii. address 0111 of P2 / iv. address 1010 of P2 | [3] / [2] / [4] / [2] / [4] |
