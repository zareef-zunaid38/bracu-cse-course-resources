---
source: CSE321_pyq_sp23_final_setB.docx (the original, not included in this repo)
fidelity: the summary above the divider is paraphrased and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 1 embedded image(s); see the summary or the source)
source-mtime: 2025-01-31T20:04:50
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Final Examination Spring 2023 Set B

CSE321 Operating Systems Spring 2023 Set B Final exam covering semaphore washroom synchronization, Peterson's algorithm execution timeline, Banker's algorithm safe sequence and request analysis, Resource Allocation Graph cycle detection, fixed-partition memory allocation (first-fit/best-fit), dynamic memory compaction, paging frame/internal fragmentation calculations, page table mapping, and page replacement (FIFO vs OPT).
Docx last modified: 31-01-2025.

## Content

### Exam Metadata
- **Institution**: BRAC University, Department of Computer Science and Engineering
- **Course**: CSE 321: Operating Systems
- **Examination**: Final Examination (Set B)
- **Semester**: Spring 2023
- **Duration**: 2 Hours
- **Total Marks**: 40

---

### Question 1 [CO4] [Marks: 3 + 4 = 7]
- **a)** In a restaurant, there are 3 washrooms available for male guests and 1 washroom available for female guests. There is a strict rule maintained by the authorities that neither men can use the female washroom nor women can use any of the male washrooms. On a random day during lunchtime, 4 female guests needed to use washroom facilities at the very same time. But none of them were allowed to use any of the male washrooms although two of them were vacant. Therefore, only 1 guest was able to get access to the washroom at a time and others had to wait while maintaining a queue. If the washroom gets vacant, a guest from the queue can get access to that. Logically explain which synchronization method has been used here. [3]

- **b)** For Peterson's problem below conditions will be applied: [4]
  - There are two processes: P1 and P2.
  - Each Statement takes 5ms to execute, P1 gets executed first.
  - Context Switch will occur after 20ms.
  - Critical section contains 2 statements.
  - Remainder section contains 4 statements.
  - For P1: `i = 0` and `j = 1`
  - For P2: `i = 1` and `j = 0`
  - Initial values: `turn = 0`, `flag[0] = FALSE`, `flag[1] = TRUE`

  The structure of process $P_i$ in Peterson's solution:
  ```c
  do {
      flag[i] = true;
      turn = j;
      while (flag[j] == true && turn == 1) {
          // busy wait
      }
      // critical section
      flag[i] = false;
      // remainder section
  } while (true);
  ```
  Complete the timeline execution table for processes P1 and P2 using Peterson's solution.

---

### Question 2 [CO4] [Marks: 4 + 5 + 4 = 13]
- **a)** Consider the following snapshot of a system:

  | Process | Allocation (A B C) | MAX (A B C) | Available (A B C) |
  | :--- | :--- | :--- | :--- |
  | **P0** | 5 2 3 | 9 9 8 | 7 10 5 |
  | **P1** | 3 2 0 | 9 9 10 | |
  | **P2** | 5 3 3 | 6 8 5 | |
  | **P3** | 3 0 0 | 6 7 9 | |
  | **P4** | 4 2 1 | 5 7 6 | |

  - Is the system in a safe state? Apply Banker's safety algorithm to find out the safe sequence. You need to calculate the need matrix. [4]
  - P4 requests for $(0\ 3\ 1)$, check the validity of the request. If the request is valid, does the system enter a deadlock? [1+4]

- **b)** Suppose, in a workplace, we have a set of resource types, $R = \{R1, R2, R3, R4\}$ and a set of processes, $P = \{P1, P2, P3, P4, P5\}$. $R1, R2, R3$, and $R4$ have 2, 3, 1, and 2 instances respectively. [4]
  - P1 is holding 1 instance of R1
  - P1 requests 1 instance of R4
  - P2 is holding 1 instance of R3
  - P2 requests 2 instances of R2
  - P3 requests for 1 instance of R1
  - P3 is holding 1 instance of R2
  - P4 requests for 1 instance of R4
  - P4 is holding 2 instances of R2 and 1 instance of R3
  - P5 is holding 1 instance of R4 and 1 instance of R1
  - P5 requests 1 instance of R2

  Construct a resource allocation graph for the above scenario. Mention the number of cycles found and identify whether there is a deadlock or not.

---

### Question 3 [CO5] [Marks: 6 + 2 + 2 = 10]
- **a)** Given fixed size memory partitions of `210k`, `350k`, `250k`, `190k`, `250k`, and `452k` (in order, bottom to top), apply first-fit and best-fit algorithms to place processes with the space requirement of `250k`, `425k`, `212k`, `160k`, `210k`, and `440k` (in order). Which algorithm makes the most effective use of memory? Is there any external fragmentation? [4+1+1]
- **b)** Considering in Dynamic memory management technique at a certain time the memory looks like the following figure:
  - Memory layout: `[ OS | P3 = 150K | 150K (free) | P6 = 300K | 150K (free) | P7 = 150K ]` (gray blocks are free spaces of 150K and 150K).
  - A process P5 = 150K arrives in the ready queue. How can you accommodate P5 in the memory? Justify your answer. [2]
- **c)** If the page size is 3 KB, how many frames will be needed in Main memory for a process size of 79,071 Bytes? Is there any internal fragmentation? — If yes, calculate the value. [$1\text{ KB} = 1024\text{ Bytes}$] [2]

---

### Question 4 [CO5] [Marks: 5 + 5 = 10]
- **a)** Suppose there are two processes P1 (16 B) and P2 (12 B) with a page size of 4 B. The main memory size is 40 B. The page tables for P1 and P2 are given below:

  **Page Table of P1:**
  | Page# | Frame# | Contents |
  | :--- | :--- | :--- |
  | 0 | 9 | UB2 |
  | 1 | 0 | UB8 |
  | 2 | 2 | UB1 |
  | 3 | 7 | UB7 |

  **Page Table of P2:**
  | Page# | Frame# | Contents |
  | :--- | :--- | :--- |
  | 0 | 4 | UB2 |
  | 1 | 3 | UB1 |
  | 2 | 6 | UB4 |

  - Draw the memory representations consisting of the contents for both P1 and P2. [2]
  - Find the corresponding physical addresses of the following logical addresses: Address `1010` of P1, Address `0111` of P2. [2]
  - How can you make efficient use of the main memory in this scenario? [1]

- **b)** Consider a computer with a main memory that has 4 frames and page reference string of 0–7 pages: `[5, 0, 3, 3, 2, 3, 7, 5, 2, 6, 3, 7]`. The page reference string represents the order in which the pages are accessed by a program. Apply FIFO & OPT algorithm to simulate the page replacement that occurs when the main memory can hold at most 4 pages at a time. Record the number of page faults and compare the result. Mention which algorithm performs better in this scenario. [4+1]

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets, headers/footers, footnotes, comments; list numbering, formatting and images are not reproduced -->

Department of Computer Science and Engineering

  (text box) BN

Final Examination Spring 2023

CSE 321: Operating Systems

| Duration: 2 Hours | Total Marks: 40 |

Answer the following questions.

Figures in the right margin indicate marks.

| 1. / CO4 | a) In a restaurant, there are 3 washrooms available for male guests and 1 washroom available for female guests. There is a strict rule maintained by the authorities that neither men can use the female washroom nor women can use any of the male washrooms. On a random day during lunchtime, 4 female guests needed to use washroom facilities at the very same time. But none of them were allowed to use any of the male washrooms although two of them were vacant. Therefore, only 1 guest was able to get access to the washroom at a time and others had to wait while maintaining a queue. If the washroom gets vacant, a guest from the queue can get access to that. Logically explain which synchronization method has been used here. | [3] |

|  | b) For Peterson’s problem below conditions will be applied. / There are two processes: P1 and P2. / Each Statement takes 5ms to execute, P1 gets executed first / Context Switch will occur after 20ms. / Critical section contains 2 statements. / Remainder section contains 4 statements. / For P1: i = 0 and j = 1 / For P2: i = 1 and j = 0 / turn=0 / flag[0] = FALSE, flag[1] = TRUE / The structure of process Pi in Peterson’s solution: / | do{ / flag[i] = true; / turn = j; / while(flag[j] == true && turn == 1){ / //busy wait / } / //critical section / flag[i] = false; / //remainder section / }while(true); | |  |

|  | Complete the table given below for processes P1 and P2 using Peterson’s solution. / | Process 1: i = 0, j = 1 | Process 2: i = 1, j = 0 | | [4] |

| 2. / CO4 | a) Consider the following snapshot of a system: / |  |  | Allocation |  | MAX |  | Available | / |  |  | A | B | C |  | A | B | C |  | A | B | C | / | P0 |  | 5 | 2 | 3 |  | 9 | 9 | 8 |  | 7 | 10 | 5 | / | P1 |  | 3 | 2 | 0 |  | 9 | 9 | 10 |  |  |  |  | / | P2 |  | 5 | 3 | 3 |  | 6 | 8 | 5 |  |  |  |  | / | P3 |  | 3 | 0 | 0 |  | 6 | 7 | 9 |  |  |  |  | / | P4 |  | 4 | 2 | 1 |  | 5 | 7 | 6 |  |  |  |  | |  |

|  | Is the system in a safe state? Apply Banker’s safety algorithm to find out the safe sequence. You need to calculate the need matrix. | [4] |

|  | P4 requests for (0 3 1), check the validity of the request. If the request is valid, does the system enter a deadlock? | [1+4] |

|  | b) Suppose, in a workplace, we have a set of resource types, R = {R1, R2, R3, R4} and a set of processes, P = {P1, P2, P3, P4, P5}. R1, R2, R3, and R4 have 2, 3, 1, and 2 instances respectively. / P1 is holding 1 instance of R1 / P1 requests 1 instance of R4 / P2 is holding 1 instance of R3 / P2 requests 2 instances of R2 / P3 requests for 1 instance of R1 / P3 is holding 1 instance of R2 / P4 requests for 1 instance of R4 / P4 is holding 2 instances of R2 and 1 instance of R3 / P5 is holding 1 instance of R4 and 1 instance of R1 / P5 requests 1 instance of R2 |  |

|  | Construct a resource allocation graph for the above scenario. Mention the number of cycles found and identify whether there is a deadlock or not. | [4] |

| 3. / CO5 | a) Given fixed size memory partitions of 210k, 350k,250k,190k, 250k and 452k (in order, bottom to top), apply first-fit and best-fit algorithms to place processes with the space requirement of 250k, 425k, 212k, 160k, 210 and 440k (in order). Which algorithm makes the most effective use of memory? Is there any external fragmentation? | [4+1+1] |

|  | b) Considering in Dynamic memory management technique at a certain time the memory looks like the following figure: / Where gray portions of the memory are free spaces. A process P5 = 150K arrives in the ready queue. How can you accommodate P5 in the memory? Justify your answer. | [2] |

|  | c) If the page size is 3 KB, how many frames will be needed in Main memory for a process size of 79,071 Bytes? Is there any internal fragmentation? - If yes, calculate the value. [1 KB = 1024 Bytes] | [2] |

| 4. / CO5 | a) Suppose there are two processes P1 (16 B) and P2 (12 B) with a page size of 4 B. The main memory size is 40 B. The page table for P1 and P2 are given below: /   (text box) Page# /   (text box) Frame# /   (text box) Contents /   (text box) 0 /   (text box) 4 /   (text box) UB2 /   (text box) 1 /   (text box) 3 /   (text box) UB1 /   (text box) 2 /   (text box) 6 /   (text box) UB4 /   (text box) Page table: P2 /   (text box) Page# /   (text box) Frame# /   (text box) Contents /   (text box) 0 /   (text box) 9 /   (text box) UB2 /   (text box) 1 /   (text box) 0 /   (text box) UB8 /   (text box) 2 /   (text box) 2 /   (text box) UB1 /   (text box) 3 /   (text box) 7 /   (text box) UB7 /   (text box) Page table: P1 |  |

|  | Draw the memory representations consisting of the contents for both P1 and P2. | [2] |

|  | Find the corresponding physical addresses of the following logical addresses: / Address 1010 of P1 / Address 0111 of P2 | [2] |

|  | How can you make efficient use of the main memory in this scenario? | [1] |

|  | b) Consider a computer with a main memory that has 4 frames and page reference string of 0-7 pages: [5, 0, 3, 3, 2, 3, 7, 5, 2, 6, 3, 7]. The page reference string represents the order in which the pages are accessed by a program. Apply FIFO & OPT algorithm to simulate the page replacement that occurs when the main memory can hold at most 4 pages at a time. Record the number of page faults and compare the result. Mention which algorithm performs better in this scenario. | [4+1] |
