# CSE321 Midterm Examination Spring 2023 Set A

CSE321 Operating Systems Spring 2023 Set A Midterm exam covering main memory volatility, monolithic vs microkernel architecture, wait() system call, fork() array mutation output tracing, Round Robin and SRTF CPU scheduling, Amdahl's Law multicore speedup, and data parallelism.
Docx last modified: 31-01-2025.

## Content

### Exam Metadata
- **Institution**: BRAC University, Department of Computer Science and Engineering
- **Course**: CSE 321: Operating Systems
- **Examination**: Midterm Examination (Set A)
- **Semester**: Spring 2023
- **Duration**: 1 Hour 15 Minutes
- **Total Marks**: 25

---

### Question 1 [CO1] [Marks: 2 + 3 + 2 + 3 = 10]
- **a)** Explain why main memories are usually volatile. [2]
- **b)** Distinguish between monolithic and microkernel OS structures. [3]
- **c)** A program has a process that will allow its child process to complete first. Which system call can be used in this scenario? Explain what may happen in absence of this system call. [2]
- **d)** Find the output of the following code snippet: [3]
  ```c
  const int len = 2;
  int main() {
      int id;
      int a[] = {10, 7};
      int b = len - 1;
      id = fork();
      if (id < 0) {
          printf("fork failed\n");
      }
      else if (id == 0) {
          printf("child process executing\n");
      }
      else {
          wait(NULL);
          printf("parent process executing\n");
          a[b - 1] = a[b - 1] - 2;
          a[b] = a[b] + 2;
      }
      for (int i = 0; i < len; i++) {
          printf("value of a[%d]: %d\n", i, a[i]);
      }
      return 0;
  }
  ```

---

### Question 2 [CO2] [Marks: 5 + 4 + 1 = 10]
Consider the following processes with arrival times and burst times:

| Processes | Arrival Time | Burst Time |
| :--- | :--- | :--- |
| **P1** | 0 | 11 |
| **P2** | 20 | 6 |
| **P3** | 14 | 9 |
| **P4** | 20 | 8 |
| **P5** | 15 | 8 |
| **P6** | 16 | 8 |
| **P7** | 2 | 2 |

- **a)** Draw a Gantt chart and illustrate the execution of the process using the Round Robin scheduling algorithm (time quantum = 5 units). Calculate the average waiting and turnaround time. [3+2]
- **b)** Apply Shortest Remaining Time First (SRTF) scheduling algorithm. Draw the Gantt chart and Calculate the average waiting and turnaround time. [2+2]
- **c)** Compare the results and identify the most suitable scheduling algorithm in this scenario. [1]

---

### Question 3 [CO3] [Marks: 2 + 3 = 5]
- **a)** A system has processes to execute of which 35% is parallel. If the number of cores is increased from 3 to 5, Explain what will be the increase/decrease in performance? [2]
- **b)** A program has multiple threads that need to perform a task which involves similar computation on a large scale of data. Identify which kind of parallelism can be applied in this scenario. Provide proper justification with a real-life example of the scenario mentioned above. [3]
