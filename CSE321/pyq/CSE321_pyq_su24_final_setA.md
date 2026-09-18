# CSE321 Final Examination Summer 2024 Set A — sidecar (read this, not the .docx)

Past exam question paper for CSE321 Operating Systems Final Examination Summer 2024 (Set A), covering fork processes, Peterson's synchronization, CPU scheduling, memory partitioning, TLB effective access time, paging address translation, and page replacement.
Docx last modified: 09-05-2025.

## Content

### Exam Metadata
- **Institution:** Department of Computer Science and Engineering, BRAC University
- **Exam:** Final Examination Summer 2024 (Set A)
- **Course:** CSE 321: Operating Systems
- **Duration:** 1 Hour 40 Minutes
- **Total Marks:** 45
- **Instructions:** Answer the following questions. Figures in the right margin indicate marks.

---

### Question 1 (CO2) [10 Marks]

#### a) [4 Marks]
Find outputs of the given code:

```c
int main(){
    pid_t p,q;
    int y[]={17,28};
    p=fork();
    if(p<0){
        printf("fork failed\n");
    }
    else if(p>0){
        y[0]=y[0]*5;
        y[1]=y[1]/2;
        wait();
        y[0]=y[0]+y[1];
        y[1]=y[1]-y[0];
    }
    else{
        y[0]=y[0]+5;
        y[1]=y[1]-5;
    }
    q=fork();
    if(q<0){
        printf("fork failed\n");
    }
    else if(q==0){
        y[0]=y[0]%2;
        y[1]=y[1]%2;
    }
    else{
        wait();
        y[0]=y[0]*y[0];
        y[1]=y[1]*y[1];
    }
    for(int j=0;j<2;j++){
        printf("y[%d] = %d\n",j,y[j]);
    }
    return 0;
}
```

#### b) [6 Marks]
Potato Bank Limited has two employees, Shoumo and Dipu who are responsible for maintaining the balance of a shared account. The system allows Shoumo and Dipu to either deposit or withdraw money from the account. To ensure the integrity of the account balance, the system needs a synchronization mechanism so that only one employee can modify the account balance at any time. Shoumo and Dipu perform 5 operations each on the bank account having a starting balance of 2200 taka. After performing operations on the account, both the employees get feedback from the customer which is equivalent to 3 operations. The operations Shoumo and Dipu performs are given below:

| Shoumo | Dipu |
| :--- | :--- |
| Deposit 1000 | Withdraw 1000 |
| Withdraw 2000 | Withdraw 500 |
| Deposit 5500 | Deposit 3000 |
| Withdraw 1100 | Withdraw 500 |
| Withdraw 1200 | Deposit 300 |

Now apply Peterson’s solution to simulate the above operations so that the integrity of the account balance is preserved. Show the account balance after each operation. The constraints and initial condition of the system is below:
- Each operation takes 4s to complete.
- Each employee must hand over the system access to the other employee every 12s.
- Shoumo ($i=1, j=0$).
- Dipu ($i=0, j=1$).
- `turn = 1`.
- `flag[0] = true`, `flag[1] = true`.
- Dipu starts working first.

The algorithm for Peterson’s solution is provided below:
```c
do {
    flag[i] = true;
    turn = j;
    while (flag[j] && turn == j);
    // Critical Section
    flag[i] = false;
    // Remainder section
} while(true);
```

---

### Question 2 (CO1) [11 Marks]

#### Given Process Table
| Processes | Arrival time | Burst time | Priority |
| :--- | :--- | :--- | :--- |
| P1 | 41 | 15 | 3 |
| P2 | 60 | 12 | 1 |
| P3 | 1 | 22 | 3 |
| P4 | 57 | 20 | 4 |
| P5 | 18 | 15 | 2 |
| P6 | 59 | 19 | 1 |

Consider the set of processes above, with the length of the CPU burst times given in milliseconds.
- Draw the Gantt charts illustrating the execution of these processes using **preemptive priority** (smaller number implies a higher priority) and **Round Robin** (time quantum = 8 ms) scheduling. [3 + 3 Marks]
- Calculate average waiting time and average turnaround time for the above scheduling algorithms. [2 + 2 Marks]
- Identify which algorithm is the best with proper justification. [1 Mark]

---

### Question 3 (CO3) [6 Marks]

#### a) [2 + 2 + 2 = 6 Marks]
Given memory partitions of 525k, 655k, 425k, 244k, 305k and 855k (in order, bottom to top). Apply **first fit** and **best fit** algorithms to place processes with space requirements of 422k, 615k, 420k, 515k and 395k. A constraint is being maintained which is that only one process can be allocated in a partition and in the main memory lower to higher memory addresses are arranged in top to bottom order. Explain which algorithm makes the most effective use of memory and find whether there is any external fragmentation or not.

#### b) [2 Marks]
In a memory system, each frame has a size of 3 KB. Placing a process with 4 pages results in an internal fragmentation of 321 Bytes. Find the size of the process in Bytes. [1 KB = 1024 Bytes]

#### c) [2 Marks]
Consider a system with TLB miss ratio of 25%, and TLB search time of 15 ns. If Effective Access Time (EAT) is 161.25 ns, find the memory access time.

---

### Question 4 (CO3) [14 Marks]

#### a) [5 + 2 = 7 Marks]
In a system the size of the main memory is 60 Bytes and size of each page is 4 Bytes. In a certain moment CPU generates logical addresses 10, 1, 15, 26 and 7 of a process respectively. Logical memory and page table of the process is given below:

**Logical Memory:**
- $P_0$: `ABCD` (Bytes 0–3)
- $P_1$: `EFGH` (Bytes 4–7)
- $P_2$: `IJKL` (Bytes 8–11)
- $P_3$: `MNOP` (Bytes 12–15)

**PMT (Page Map Table):**
| Page | Frame |
| :--- | :--- |
| $P_0$ | 11 |
| $P_1$ | 6 |
| $P_2$ | 17 |
| $P_3$ | 2 |

- Find corresponding physical addresses of generated logical addresses (10, 1, 15, 26, 7). If the physical address is not valid then write "invalid". [5 Marks]
- Map user’s view of the main memory. [2 Marks]

#### b) [3 + 3 + 1 = 7 Marks]
Consider a computer with a main memory that has 3 frames and page reference string of 0–6 pages:
`[5, 4, 5, 2, 5, 1, 1, 6, 2, 0, 4, 0]`

The page reference string represents the order in which the pages are accessed by a program. Apply **FIFO** & **LRU** algorithm to simulate the page replacement that occurs when the main memory can hold at most 3 pages at a time and record the number of page faults. Now compare results and mention which algorithm performs better in this scenario.
