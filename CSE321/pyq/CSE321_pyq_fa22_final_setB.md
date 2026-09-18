# CSE 321 Final Examination Fall 2022 (Set B) — sidecar (read this, not the .docx)

Final examination question paper for CSE321 Fall 2022 Set B covering Amdahl's Law, multithreading models, Peterson's solution, Banker's Algorithm, RAG deadlock analysis, dynamic memory partitioning, paging, MMU, and page replacement.
Docx last modified: 31-01-2025.

## Content

**Course**: CSE 321: Operating Systems  
**Semester**: Fall 2022  
**Exam**: Final Examination (Set B)  
**Duration**: 2 Hours | **Total Marks**: 40  

---

### Question 1 [CO4] (Total: 4 Marks)
- **a)** [2 Marks] A system has processes to execute of which 30% is serial. If the number of cores is increased from 1 to 3, what will be the increase in performance?
- **b)** [2 Marks] Distinguish between many-to-one and one-to-one multithreading models.

---

### Question 2 [CO5] (Total: 6 Marks)
- **a)** [2 Marks] Suppose a medical center is providing Covid vaccination. In that center maximum of 6 people can take vaccines at a time in separate booths. But approximately 50 people went there to take vaccines on a particular day. Therefore, the authorities have decided that they will provide vaccines to 6 people at a time and keep others waiting in a queue. If any of the vaccine booths get free a person from the queue will be taken to that booth according to the first come first serve manner for vaccination. Logically explain which synchronization method has been used here.
- **b)** [4 Marks] For Peterson’s problem below conditions will be applied:
  - There are two processes: P1 and P2.
  - Each Statement takes 4ms to execute.
  - Context Switch will occur after 12ms.
  - Both the Critical & Remainder section contains 3 statements.
  - For P1: $i=0$ and $j=1$. For P2: $i=1$ and $j=0$.
  - `turn = 0`, `flag[0] = FALSE`, `flag[1] = FALSE`.
  - Structure of process $P_i$ in Peterson’s solution:
    ```c
    do {
        flag[i] = true;
        turn = j;
        while(flag[j] == true && turn == j) {
            // busy wait
        }
        // critical section
        flag[i] = false;
        // remainder section
    } while(true);
    ```
  - Complete the execution timeline table for processes P1 and P2 using Peterson’s solution:
    - Process 1: $i=0, j=1$
    - Process 2: $i=1, j=0$

---

### Question 3 [CO5] (Total: 12 Marks)
- **a)** [4 + 5 Marks] Consider the following snapshot of a system:

| Process | Allocation (A B C D) | MAX (A B C D) | Available (A B C D) |
| :--- | :--- | :--- | :--- |
| **P0** | 5 1 1 4 | 10 5 10 11 | **4 4 6 4** |
| **P1** | 2 8 6 4 | 9 12 9 10 | |
| **P2** | 2 2 6 2 | 5 9 10 3 | |
| **P3** | 4 6 8 2 | 4 6 8 3 | |

  1. Is the system in a safe state? Apply Banker’s safety algorithm to find out the safe sequence. You need to calculate the need matrix.
  2. P0 requests for $(3, 3, 6, 1)$, check the validity of the request. If the request is valid, does the system enter a deadlock?

- **b)** [3 Marks] Suppose, in an office, we have a set of resource types, $R = \{R1, R2, R3\}$ and a set of processes, $P = \{P1, P2, P3, P4, P5\}$. R1, R2, and R3 have 3, 2, and 1 instance respectively.
  - P1 is holding 1 instance of R3
  - P1 requests 1 instance of R1
  - P3 requests for 1 instance of R3
  - P3 is holding 2 instances of R2
  - P2 requests for 1 instance of R2
  - P2 is holding 1 instance of R1
  - P4 is holding 1 instance of R1
  - P4 requests 1 instance of R2
  - P5 is holding 1 instance of R1
  - **Task**: Construct a resource allocation graph for the above scenario. Mention the number of cycles found and identify whether there is a deadlock or not.

---

### Question 4 [CO6] (Total: 12 Marks)
- **a)** [2 + 2 + 1 Marks] At a particular time, the snapshot of Main memory dynamic partitions is given (left to right):
  - `400K (Free)` | `300K (Occupied/Gray)` | `160K (Free)` | `50K (Occupied/Gray)` | `200K (Free)` | `260K (Free)`
  - Apply **worst fit** and **first fit** algorithms to place processes with space requirements of:
    $P_1=300\text{k}, P_2=200\text{k}, P_3=149\text{k}, P_4=146\text{k}, P_5=100\text{k}, P_6=50\text{k}, P_7=22\text{k}, P_8=29\text{k}$ (in order).
  - Explain which algorithm makes the most effective use of memory.
- **b)** [2 Marks] Assume that in a paged memory management system the page size for processes is 4 bytes and the Physical Memory size is 36 bytes. Show the users’ view of memory which is mapped into physical memory:
  - Logical space of a process: P0 (Free), P1 (Finite), P2 (Fruit), P3 (From), P4 (Flower), P5 (Freedom).
  - Page Map Table (PMT): $0 \to 2, 1 \to 7, 2 \to 4, 3 \to 5, 4 \to 3, 5 \to 1$.
- **c)** [2 Marks] If the page size is 6 KB, how many frames will be needed in Main memory for a process size of 102,506 Bytes? Is there any internal fragmentation? - If yes, calculate the value ($1\text{ KB} = 1024\text{ Bytes}$).
- **d)** [3 Marks] Discuss the purpose of MMU (Memory Management Unit).

---

### Question 5 [CO6] (Total: 6 Marks)
Consider a computer with a main memory that has 3 frames and page reference string of 0-7 pages:
`[ 3, 0, 6, 4, 2, 6, 7, 2, 0, 1, 7 ]`  
The page reference string represents the order in which the pages are accessed by a program. Apply **FIFO** & **LRU** algorithm to simulate the page replacement that occurs when the main memory can hold at most 3 pages at a time. Record the number of page faults and compare the result. Mention which algorithm performs better in this scenario.\n