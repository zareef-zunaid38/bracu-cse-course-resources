# CSE321 Final Examination Summer 2023 Set A — sidecar (read this, not the .docx)

CSE321 Operating Systems Summer 2023 Set A Final exam covering printer counting semaphore synchronization, busy waiting drawbacks, Peterson's algorithm execution timeline tracing, Banker's algorithm safe sequence and request verification, Resource Allocation Graph deadlock analysis, dynamic memory partitioning (best-fit/worst-fit), paging throughput, cache/TLB Effective Access Time comparison, page table logical to physical address conversion, and page replacement algorithms (LRU vs OPT).
Docx last modified: 31-01-2025.

## Content

### Exam Metadata
- **Institution**: BRAC University, Department of Computer Science and Engineering
- **Course**: CSE 321: Operating Systems
- **Examination**: Final Examination (Set A)
- **Semester**: Summer 2023
- **Duration**: 1 Hour 50 Minutes
- **Total Marks**: 40

---

### Question 1 [CO4] [Marks: 3 + 1.5 + 3.5 = 8]
- **a)** Imagine a computer lab with multiple computers, and equipped with printers. Students use these computers for various tasks and may want to print documents. However, there are only a limited number of printers available. How do you solve this issue using semaphore? Your answer should have the steps associated in solving the given scenario. [3]
- **b)** What is meant by busy waiting in the implementation of a critical section solution, and why is it considered problematic? [1.5]
- **c)** For Peterson's problem below conditions will be applied: [3.5]
  - There are two processes: P1 and P2. P2 gets to execute first.
  - Each Statement takes 4ms to execute.
  - Context Switch will occur after 12ms.
  - Critical section contains 4 statements.
  - Remainder section contains 3 statements.
  - For P1: `i=0` and `j=1`
  - For P2: `i=1` and `j=0`
  - Initial values: `turn=0`, `flag[0] = FALSE`, `flag[1] = TRUE`

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

  | Process | Max (A B C D) | Allocation (A B C D) | Available (A B C D) |
  | :--- | :--- | :--- | :--- |
  | **P1** | 7 0 1 3 | 7 0 0 2 | 1 6 4 4 |
  | **P2** | 2 7 5 0 | 2 1 0 0 | |
  | **P3** | 2 12 5 6 | 0 6 3 3 | |
  | **P4** | 1 6 5 6 | 0 2 1 2 | |

  - Calculate the Need Matrix. Is this system in a safe state? If yes, then find the safe sequence using Banker's Safety algorithm; otherwise, provide the necessary explanation. [1+3]
  - What happens if the process P4 requests at this moment for $(0, 4, 2, 0)$? Does Banker's algorithm grant the request or not? [5]

- **b)** Suppose, in a workplace, we have a set of resource types, $R = \{R1, R2, R3, R4\}$ and a set of processes, $P = \{P1, P2, P3, P4, P5\}$. $R1, R2, R3$, and $R4$ have 3, 2, 3, and 2 instances respectively. [4]
  - P1 is holding 2 instances of R1
  - P2 is holding 1 instance of R3
  - P3 is holding 1 instance of R4
  - P5 requests 2 instances of R3
  - P4 is holding 1 instance of R4
  - P3 requests 1 instance of R2
  - P2 requests 1 instance of R1
  - P2 is holding 1 instance of R2
  - P1 is requesting 1 instance of R4
  - P3 is holding 1 instance of R3
  - P4 is holding 1 instance of R2

  Construct a resource allocation graph for the above scenario and identify the cycle (if any) and decide whether there is a deadlock or not.

---

### Question 3 [CO5] [Marks: 5 + 3 + 3 = 11]
- **a)** Given variable size memory (dynamic) partitions of 8 MB, 29 MB, 35 MB, and 48 MB (in order, top to bottom), apply best-fit and worst-fit algorithms to place processes with the space requirement of 5 MB, 15 MB, 10 MB, 5 MB, 10 MB, 20 MB, 25 MB, and 15 MB (in order). Which algorithm makes the most effective use of memory? [4+1]
- **b)** How is paging efficiently used in main memory to increase throughput? Justify with necessary examples. [3]
- **c)** Compare the following systems in terms of performance: [3]
  - **1.** A system with a hit ratio of 93%, associative lookup time of 32ns, and memory access time of 72ns.
  - **2.** A system with a hit ratio of 62%, associative lookup time of 9ns, and memory access time of 133ns.

---

### Question 4 [CO5] [Marks: 3 + 5 = 8]
- **a)** Suppose there are two processes P1 (32 B) and P2 (40 B) with a page size of 8 B. The main memory size is 96 B. Find the corresponding physical addresses of the following logical addresses: [3]
  - Address `011010` of P1
  - Address `111110` of P2
  - Address `011001` of P2
- **b)** Consider a computer with a main memory that has 5 frames and page reference string of 0–7 pages: `[2, 2, 2, 6, 5, 5, 4, 2, 0, 0, 6, 1, 5, 5, 3, 0, 0, 2, 6, 5]`. The page reference string represents the order in which the pages are accessed by a program. Apply LRU & OPT algorithm to simulate the page replacement that occurs when the main memory can hold at most 5 pages at a time. Record the number of page faults and compare the result. Mention which algorithm performs better in this scenario. [4+1]
