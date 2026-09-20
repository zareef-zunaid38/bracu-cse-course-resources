# CSE321 Midterm Examination Summer 2023 Set A

CSE321 Operating Systems Summer 2023 Set A Midterm exam covering cooperating processes and IPC models, multiprogrammed OS architecture drawbacks, privileged instructions identification, fork() and wait() arithmetic output tracing, Round Robin and Preemptive Priority CPU scheduling, data parallelism, multithreading for real-time ADAS embedded systems, and Amdahl's Law performance calculation.
Docx last modified: 31-01-2025.

## Content

### Exam Metadata
- **Institution**: BRAC University, Department of Computer Science and Engineering
- **Course**: CSE 321: Operating Systems
- **Examination**: Midterm Examination (Set A)
- **Semester**: Summer 2023
- **Duration**: 1 Hour 15 Minutes
- **Total Marks**: 25

---

### Question 1 [CO1] [Marks: 3 + 2 + 2 + 3 = 10]
- **a)** Why do we need cooperating processes, and what are the two models of interprocess communication? Additionally, mention the strengths and weaknesses of each approach? [3]
- **b)** Discuss one drawback faced by multi-programmed OS architecture and suggest possible way(s) to overcome it. [2]
- **c)** What is the purpose of system calls? Which of the following instructions should be privileged? [1+1]
  - i. Set value of timer
  - ii. Read the clock
  - iii. Clear memory
  - iv. Issue a trap instruction
  - v. Turn off interrupts
  - vi. Modify entries in device-status table
- **d)** Find the output of the following code snippet: [3]
  ```c
  int main() {
      pid_t child_pid;
      int global_a = 90, b = 11;
      char message[] = "Hello, from the ";
      printf("Parent process started\n");
      child_pid = fork();
      if (child_pid == -1) {
          printf("Fork Failed\n");
      } else if (child_pid > 0) {
          wait(NULL);
          global_a += 97;
          printf("%sAddition: %d + %d = %d\n", message, global_a, b, global_a);
          printf("%sSubtraction: %d - %d = %d\n", message, b, global_a, b);
      } else {
          b *= 7;
          printf("Multiplication: %d * %d = %d\n", global_a, b, global_a);
          printf("Division: %d / %d = %d\n", b, global_a, b);
      }
      return 0;
  }
  ```

---

### Question 2 [CO2] [Marks: 5 + 4 + 1 = 10]
Consider the following processes arriving in the ready queue:

| Processes | Arrival Time | Burst Time | Priority |
| :--- | :--- | :--- | :--- |
| **P1** | 0 | 4 | 2 |
| **P2** | 6 | 4 | 1 |
| **P3** | 7 | 6 | 6 |
| **P4** | 7 | 1 | 3 |
| **P5** | 8 | 7 | 4 |
| **P6** | 19 | 7 | 5 |

- **a)** Draw a Gantt chart and illustrate the execution of the process using the Round Robin scheduling algorithm (time quantum = 5 units). Calculate the average waiting and turnaround time. [3+2]
- **b)** Apply Preemptive Priority scheduling algorithm. Draw the Gantt chart and Calculate the average waiting and turnaround time. [2+2]
- **c)** Compare the results and identify the most suitable scheduling algorithm in this scenario. [1]

---

### Question 3 [CO3] [Marks: 1.5 + 1.5 + 2 = 5]
- **a)** Explain data parallelism with an example. [1.5]
- **b)** You are developing a real-time embedded system for a safety-critical application, such as an advanced driver assistance system (ADAS) for autonomous vehicles. The system's primary goal is to process sensor data, make critical decisions, and take actions in real-time to ensure the safety of passengers and pedestrians. However, you may assume there's no limitation of computational resources on the project you are working on. Based on the scenario, which multi-threading model would you recommend for implementation? Provide necessary justification. [1.5]
- **c)** A system has processes to execute of which 30% is serial. If the number of cores is decreased from 9 to 4, Explain the change in the performance. [2]
