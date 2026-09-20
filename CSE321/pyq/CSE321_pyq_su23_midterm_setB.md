---
source: CSE321_pyq_su23_midterm_setB.docx (the original, not included in this repo)
fidelity: the summary above the divider is paraphrased and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 1 embedded image(s); see the summary or the source)
source-mtime: 2025-02-22T16:05:52
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Midterm Examination Summer 2023 Set B

Past exam question paper for CSE321 Operating Systems Midterm Examination Summer 2023 (Set B), covering OS fundamentals, schedulers, dual-mode operations, fork output tracing, CPU scheduling, task parallelism, multithreading models, and Amdahl's Law.
Docx last modified: 22-02-2025.

## Content

### Exam Metadata
- **Institution:** Department of Computer Science and Engineering, BRAC University
- **Exam:** Midterm Examination Summer 2023 (Set B)
- **Course:** CSE 321: Operating Systems
- **Duration:** 1 Hour 15 Minutes
- **Total Marks:** 25
- **Instructions:** Answer the following questions. Figures in the right margin indicate marks.

---

### Question 1 (CO1) [10 Marks]

#### a) [3 Marks]
Define the term "interrupt" in the context of operating systems. Mention one advantage and one disadvantage of a multiprocessor system.

#### b) [2 Marks]
A process from its creation till its completion will go through various states. To enter different states, the process requires the decision of different types of scheduler. State the name of different schedulers for different process states with justification.

#### c) [1 + 1 = 2 Marks]
What is the purpose of dual mode operation? Which of the following instructions should be privileged?
i. Access I/O device  
ii. Set value of timer  
iii. Read the date in the calendar  
iv. Clear memory  
v. Switch from user to kernel mode  
vi. Turn off interrupts  

#### d) [3 Marks]
Find the output of the following code snippet. Your output should exactly match with the original output.

```c
int main() {
    pid_t child_pid;
    int global_a = 68, b = 10;
    char message[] = "Hello, from the ";
    printf("Parent process started\n");
    child_pid = fork();
    if (child_pid == -1) {
        printf("Fork Failed\n");
    } else if (child_pid > 0) {
        wait(NULL);
        b *= 38;
        printf("Multiplication: %d * %d = %d\n", global_a, b, global_a);
        printf("Division: %d / %d = %d\n", b, global_a, b);
    } else {
        global_a += 98;
        printf("%sAddition: %d + %d = %d\n", message, global_a, b, global_a);
        printf("%sSubtraction: %d - %d = %d\n", message, b, global_a, b);
    }
    return 0;
}
```

---

### Question 2 (CO2) [10 Marks]

#### Given Process Table
| Processes | Arrival Time | Burst Time | Priority |
| :--- | :--- | :--- | :--- |
| P1 | 0 | 4 | 2 |
| P2 | 6 | 4 | 1 |
| P3 | 7 | 6 | 6 |
| P4 | 7 | 1 | 3 |
| P5 | 8 | 7 | 4 |
| P6 | 19 | 7 | 5 |

#### a) [3 + 2 = 5 Marks]
Draw a Gantt chart and illustrate the execution of the process using the Round Robin scheduling algorithm (time quantum = 5 units). Calculate the average waiting and turnaround time.

#### b) [2 + 2 = 4 Marks]
Apply Preemptive Priority scheduling algorithm (smaller number implies a higher priority). Draw the Gantt chart and calculate the average waiting and turnaround time.

#### c) [1 Mark]
Compare the results and identify the most suitable scheduling algorithm in this scenario.

---

### Question 3 (CO3) [5 Marks]

#### a) [1.5 Marks]
Explain task parallelism with an example.

#### b) [1.5 Marks]
You are developing a lightweight, user-level threading library for a resource-constrained embedded system. The embedded system has limited processing power and memory, and it does not provide native support for multithreading at the kernel level. The primary goal is to allow concurrent execution of multiple tasks while minimizing the overhead of managing threads.  
Based on the scenario, which multi-threading model would you recommend for implementation? Provide necessary justification.

#### c) [2 Marks]
A system has processes to execute of which 32% is serial. If the number of cores is decreased from 8 to 2, explain the change in the performance (using Amdahl's Law).

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, headers/footers, footnotes, comments, link targets; formatting and images are not reproduced -->

Department of Computer Science and Engineering

  (text box) B

Midterm Examination Summer 2023

CSE 321: Operating Systems

| Duration: 1 Hour 15 Minutes | Total Marks: 25 |

Answer the following questions.

Figures in the right margin indicate marks.

| 1. / CO1 | a) Define the term "interrupt" in the context of operating systems. Mention one advantages and one disadvantage of a multiprocessor system. | [3] |

|  | b) A process from its creation till its completion will go through various states. To enter different states, the process requires the decision of different types of scheduler. State the name of different schedulers for different process states with justification. | [2] |

|  | c) What is the purpose of dual mode operation? Which of the following instructions should be privileged? / i. Access I/O device ii. Set value of timer iii. Read the date in the calendar. / iv. Clear memory v. Switch from user to kernel mode vi. Turn off interrupts. | [1+1] |

|  | d) Find the output of the following code snippet. Your output should exactly match with the original output. / | int main() { / pid_t child_pid; / int global_a = 68, b = 10; / char message[] = "Hello, from the "; / printf("Parent process started\n"); / child_pid = fork(); / if (child_pid == -1) { / printf(“Fork Failed\n”); / } else if (child_pid > 0) { / wait(NULL); / b *= 38; / printf("Multiplication: %d * %d = %d\n", global_a, b, global_a); / printf("Division: %d / %d = %d\n", b, global_a, b); / } else { / global_a += 98; / printf("%sAddition: %d + %d = %d\n", message, global_a, b, global_a); / printf("%sSubtraction: %d - %d = %d\n", message, b, global_a, b); / } / return 0; / } | | [3] |

| 2. / CO2 | | Processes | Arrival Time | Burst Time | Priority | / | P1 | 0 | 4 | 2 | / | P2 | 6 | 4 | 1 | / | P3 | 7 | 6 | 6 | / | P4 | 7 | 1 | 3 | / | P5 | 8 | 7 | 4 | / | P6 | 19 | 7 | 5 | |  |

|  | a) Draw a Gantt chart and illustrate the execution of the process using the Round Robin scheduling algorithm (time quantum = 5 units). Calculate the average waiting and turnaround time. | [3+2] |

|  | b) Apply Preemptive Priority scheduling algorithm. Draw the Gantt chart and Calculate the average waiting and turnaround time. | [2+2] |

|  | c) Compare the results and identify the most suitable scheduling algorithm in this scenario. | [1] |

| 3. / CO3 | a) Explain task parallelism with an example. | [1.5] |

|  | b) You are developing a lightweight, user-level threading library for a resource-constrained embedded system. The embedded system has limited processing power and memory, and it does not provide native support for multithreading at the kernel level. The primary goal is to allow concurrent execution of multiple tasks while minimizing the overhead of managing threads. / Based on the scenario, which multi-threading model would you recommend for implementation? Provide necessary justification. | [1.5] |

|  | c) A system has processes to execute of which 32% is serial. If the number of cores is decreased from 8 to 2, Explain the change in the performance. | [2] |

[footer1.xml] PAGE OF 2
