---
source: CSE321_pyq_sp24_midterm_setA.docx (the original, not included in this repo)
fidelity: the summary above the divider is paraphrased and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 2 embedded image(s); see the summary or the source)
source-mtime: 2025-02-22T15:57:46
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Midterm Examination Spring 2024 Set A

CSE321 Operating Systems Spring 2024 Set A Midterm exam covering dual-mode OS security, privileged instructions, Amdahl's Law multicore performance degradation, multithreading models (Many-to-One), Round Robin CPU scheduling and quantum analysis, fork() and wait() static variable output tracing, process states multitasking, library checkout mutex synchronization, and binary vs counting semaphores.
Docx last modified: 22-02-2025.

## Content

### Exam Metadata
- **Institution**: BRAC University, Department of Computer Science and Engineering
- **Course**: CSE 321: Operating Systems
- **Examination**: Midterm Examination (Set A)
- **Semester**: Spring 2024
- **Duration**: 1 Hour 20 Minutes
- **Total Marks**: 30

---

### Question 1 [CO1] [Marks: 2 + 2 + 3 = 7]
- **a)** How does the dual-mode operation contribute to the security and stability of an operating system? Provide an example of a privileged instruction and its significance. [2]
- **Context for (b) & (c)**: Adhara, a CSE student, was running an application that had 65% parallel portion on a multicore system. While running the application, she started reading a book on OS and learned about a multithreading model which allows only one thread to access the kernel at a time.
- **b)** If the number of cores is reduced from 7 to 5 during application execution, calculate and explain the change in performance. [2]
- **c)** Identify the multithreading model that Adhara learned of. Write down one disadvantage of the model used and briefly explain the model that overcomes that disadvantage. [3]

---

### Question 2 [CO1] [Marks: 6 + 1 = 7]
Consider the following processes arriving in the ready queue:

| Processes | Arrival Time | Burst Time |
| :--- | :--- | :--- |
| **P1** | 0 | 4 |
| **P2** | 5 | 2 |
| **P3** | 9 | 6 |
| **P4** | 9 | 8 |
| **P5** | 8 | 8 |

- **a)** Draw a Gantt chart and illustrate the execution of the process using the Round Robin scheduling algorithm (time quantum = 3 units). Calculate the average waiting time and number of context switching. [3+2+1]
- **b)** Is a time quantum of 3 justified in this scenario? State your reasoning. If not, what would have been the optimal time quantum? [1]

---

### Question 3 [CO2] [Marks: 3 + 3 = 6]
- **a)** Find the output of the following code snippet: [3]
  ```c
  const int len = 3;
  static int r1 = 0;
  static int r2 = 0;

  int main() {
      pid_t f;
      int a[] = {12, 7, 6};
      int b = len - 1;
      f = fork();
      if (f < 0) {
          printf("fork failed\n");
      }
      else if (f == 0) {
          r1 = r1 + (a[0] + a[b]);
          r2 = r2 + (a[1] - a[b]);
      }
      else {
          wait();
          r1 = r1 + (a[0] / a[b]);
          r2 = r2 + (a[1] * a[b]);
      }
      printf("r1 = %d\n", r1);
      printf("r2 = %d\n", r2);
      if (f == 0) {
          printf("child is terminating\n");
      }
      else {
          printf("parent is terminating\n");
      }
      return 0;
  }
  ```
- **b)** Describe how the concept of process states facilitates multitasking in an operating system. [3]

---

### Question 4 [CO2] [Marks: 3 + 3 + 4 = 10]
- **Context**: A university library wants to ensure that only one student can use a checkout machine at a time to prevent conflicts. Before checking out, a student ensures if the "Checkout Key" is available. If yes, then the student takes the key and inserts it into the designated slot on the checkout machine, signaling that they are currently using it. This prevents others from using the checkout machine. After completion, the student removes the Checkout Key from the machine and returns it to its original location, making it available for the next student to use.
- **a)** Identify which synchronization method has been used here? Which synchronization problems can be solved by this method? [3]
- **b)** How do binary semaphores differ from counting semaphores? Provide an example of real-life implementation for each of the methods. [3]
- **c)** In a system, following conditions are present: [4]
  - There are 3 processes: P1, P2 and P3.
  - Initialization value of the semaphore: `s = 1`.
  - Ready queue is in the following order: `[P3, P1, P2]`.
  - CPU allocation is managed by Round Robin scheduling algorithm with time quantum of 6 ms.
  - Each statement takes 2 ms to execute.
  - Critical section contains 2 statements.
  - Remainder section contains 3 statements.
  - The structure of process Pi in the solution using semaphore (figure, transcribed):
    ```c
    wait(s) {
        while (s <= 0)
            ; // busy wait
        s--;
    }

    signal(s) {
        s++;
    }

    do {
        wait(s);
        // critical section
        signal(s);
        // remainder section
    } while (true);
    ```
  - Complete the execution table for processes P1, P2, and P3 using mutex / binary semaphore.

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets, headers/footers, footnotes, comments; list numbering, formatting and images are not reproduced -->

Department of Computer Science and Engineering

  (text box) A

Midterm Examination Spring 2024

CSE 321: Operating Systems

| Duration: 1 Hour 20 Minutes | Total Marks: 30 |

Answer the following questions.

Figures in the right margin indicate marks.

| 1. / CO1 | a) How does the dual-mode operation contribute to the security and stability of an operating system? Provide an example of a privileged instructions and its significance. | [2] |

|  | Adhara, a CSE student, was running an application that had 65% parallel portion on a multicore system. While running the application, she started reading a book on OS and learned about a multithreading model which allows only one thread to access the kernel at a time. |  |

|  | b) If the number of cores is reduced from 7 to 5 during application execution, calculate and explain the change in performance. | [2] |

|  | c) Identify the multithreading model that Adhara learned of. Write down one disadvantage of the model used and briefly explain the model that overcomes that disadvantage. | [3] |

| 2. / CO1 | a) Draw a Gantt chart and illustrate the execution of the process using the Round Robin scheduling algorithm (time quantum = 3 units). Calculate the average waiting time and number of context switching. / | Processes | Arrival Time | Burst Time | / | P1 | 0 | 4 | / | P2 | 5 | 2 | / | P3 | 9 | 6 | / | P4 | 9 | 8 | / | P5 | 8 | 8 | | [3+2+1] |

|  | b) Is a time quantum of 3 justified in this scenario? State your reasoning. If not, what would have been the optimal time quantum? | [1] |

| 3. / CO2 | a) Find the output of the following code snippet. / | const int len=3; / static int r1=0; / static int r2=0; / int main(){ / pid_t f; / int a[]={12,7,6}; / int b=len-1; / f=fork(); / if(f<0){ / printf("fork failed\n"); / } / else if(f==0){ / r1=r1+(a[0]+a[b]); / r2=r2+(a[1]-a[b]); / } / else{ / wait(); / r1=r1+(a[0]/a[b]); / r2=r2+(a[1]*a[b]); / } / printf("r1 = %d\n",r1); / printf("r2 = %d\n",r2); / if(f==0){ / printf("child is terminating\n"); / } / else{ / printf("parent is terminating\n"); / } / return 0; / } | | [3] |

|  | b) Describe how the concept of process states facilitates multitasking in an operating system. | [3] |

| 4. / CO2 | A university library wants to ensure that only one student can use a checkout machine at a time to prevent conflicts. Before checking out, a student ensures if the “Checkout Key” is available. If yes, then the student takes the key and inserts it into the designated slot on the checkout machine, signaling that they are currently using it. This prevents others from using the checkout machine. After completion, the student removes the Checkout Key from the machine and returns it to its original location, making it available for the next student to use. |  |

|  | a) Identify which synchronization method has been used here? Which synchronization problems can be solved by this method? | [3] |

|  | b) How do binary semaphores differ from counting semaphores? Provide an example of real-life implementation for each of the methods. | [3] |

|  | In a system, following conditions are present. / There are 3 processes: P1, P2 and P3. / Initialization value of the semaphore, s=1. / Ready queue is in the following order, [P3, P1, P2]. / CPU allocation is managed by round robin scheduling algorithm with the time quantum of 6 ms. / Each statement takes 2 ms to execute. / Critical section contains 2 statements. / Remainder section contains 3 statements. / The structure of process Pi in solution using semaphore: |  |

|  | c) Complete the table given below for processes P1, P2 and P3 using mutex lock. / | Process 1 | Process 2 | Process 3 | | [4] |
