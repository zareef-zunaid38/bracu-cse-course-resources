# CSE321 Final Examination Summer 2022

CSE321 Operating Systems Summer 2022 Final exam covering data integrity, critical section requirements, programming contest PC resource critical section identification, Peterson's algorithm execution table tracing up to 45ms, Banker's algorithm with 5 resource types, paging vs contiguous allocation, internal fragmentation calculation, segmentation logical-to-physical address translation, static partitioning (First/Best/Worst Fit), and MVT dynamic memory compaction.
Docx last modified: 31-01-2025.

## Content

### Exam Metadata
- **Institution**: BRAC University, Department of Computer Science and Engineering
- **Course**: CSE 321: Operating Systems
- **Examination**: Final Examination
- **Semester**: Summer 2022
- **Duration**: 1 Hour 50 Minutes
- **Full Marks**: 35

---

### Question 1 [CO5] [Marks: 2 + 2 + 5 = 9]
- **a)** Explain Data integrity problem with an example. [2]
- **b)** Name the three conditions that any solution for the critical section must meet. In a programming contest each group consists of three members. But due to lack of PC collection, each group will be given one PC to write and submit program. All members can discuss or solve a problem together. All members can also see the monitor together. Identify critical section (CS) for the scenario. [1+1]
- **c)** For Peterson's problem, the conditions below will apply: [5]
  - Each statement will take 3ms to complete.
  - For process 0: `i=0, j=1`; and for process 1: `i=1, j=0`.
  - Context switching will occur after every 9ms.
  - In the critical section area, there are only 2 statements.
  - The remaining section area contains only 1 statement.
  - Information common to both processes: `turn=0; flag[0]=FALSE; flag[1]=FALSE;`
  - Complete the timeline table up to 45ms writing the corresponding lines of code each process executes in that time slot.

---

### Question 2 [CO5] [Marks: 5 + 4 = 9]
Consider the following snapshot of a system:

| Process | Allocation (A B C D E) | Max (A B C D E) | Available (A B C D E) |
| :--- | :--- | :--- | :--- |
| **T0** | 7 0 0 2 1 | 7 0 1 3 2 | 1 5 4 4 5 |
| **T1** | 2 1 0 0 1 | 2 7 5 0 4 | |
| **T2** | 0 6 3 3 1 | 2 8 5 6 1 | |
| **T3** | 0 2 1 2 1 | 1 6 4 6 3 | |
| **T4** | 0 1 0 0 2 | 2 2 1 0 5 | |

- **i)** Using the Banker's Safety algorithm, determine whether or not the state is unsafe. If the state is safe, illustrate the order in which the processes may complete. Otherwise, illustrate why the state is unsafe. You must calculate the Need matrix. [3+2]
- **ii)** If a request from process T1 arrives for $(0, 4, 2, 0, 2)$, can the request be granted immediately? Explain with proper calculations. [4]

---

### Question 3 [CO6] [Marks: 4 + 2 + 5 + 1 + 2 + 3 = 17]
- **a)** Explain why Paging is more beneficial than Contiguous allocation. If page size is 2 KB, then how many frames will be needed in Main memory for a process size of 73,506 Bytes? Is there any internal fragmentation? — If yes, calculate the value. [2+2]
- **b)** Explain how the operating systems perform and hardware mechanism for logical to physical address translation for Segmentation. [2]
- **c)** Consider a static partitioned allocation with partition size `200`, `250`, `450`, `160`, `320`, `150`, and `600` units. You have to load five processes into this partitioned physical memory of the system. Space requirements of these processes are `146`, `425`, `240`, `89`, and `450` units. Show the allocation of these processes using First Fit, Best Fit, and Worst Fit algorithms. [5]
- **d)** Identify which algorithm is better based on memory usage. [1]
- **e)** Find if there has been any external fragmentation in this scenario? Explain for 'yes' or 'no'. [2]
- **f)** Considering MVT memory management technique at a certain time the memory looks like the following figure:
  - Memory layout: `[ OS | J5=50k | 10k (free) | J4=70k | 30k (free) | J3=70k | 26k (free) ]`
  - A job J6=55K arrives in the ready queue. Can you accommodate J6 in the memory? Justify your answer. If not, then what can be done to accommodate the new job? [3]
