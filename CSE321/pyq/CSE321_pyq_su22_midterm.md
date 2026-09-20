---
source: CSE321_pyq_su22_midterm.docx (the original, not included in this repo)
fidelity: the summary above the divider is paraphrased and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 1 embedded image(s); see the summary or the source)
source-mtime: 2025-01-31T20:04:50
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Midterm Examination Summer 2022

CSE321 Operating Systems Summer 2022 Midterm exam covering multiprogramming concepts, User vs Kernel modes, system calls and System Call Interface, thread benefits and computing acceleration, Amdahl's Law speedup, fork() process creation output tracing, CPU scheduling comparison (Priority vs Round Robin with q=4), and starvation solutions.
Docx last modified: 31-01-2025.

## Content

### Exam Metadata
- **Institution**: BRAC University, Department of Computer Science and Engineering
- **Course**: CSE 321: Operating Systems
- **Examination**: Midterm Examination
- **Semester**: Summer 2022
- **Duration**: 1 Hour 15 min
- **Total Marks**: 30

---

### Question 1 [CO1, CO2] [Marks: 3 + 4 + 3 = 10]
- **a)** What is multiprogramming? What are the requirements for multiprogramming? [3]
- **b)** Explain User mode and Kernel mode execution with example. [4]
- **c)** Define system call? Explain how system call works using System Call Interface. [3]

---

### Question 2 [CO2] [Marks: 3 + 3 + 4 = 10]
- **a)** Though we can increase the hardware resources as much as we want by money, still why do we implement threads? Explain how threads accelerate computing power with an example. [3]
- **b)** A system has processes to execute of which 40% is parallel. If the number of cores is increased from 2 to 4, what will be the increase in performance? [3]
- **c)** Find the output of the following code: [4]
  ```c
  int main() {
      int id, i;
      printf("Start of main...\n");
      id = fork();
      if (id > 0) {
          printf("Parent section...\n");
      }
      else if (id == 0) {
          printf("\n fork created...\n");
      }
      else {
          printf("\n fork creation failed!!!\n");
      }
      printf("Printing the numbers from 1 to 10\n");
      for (i = 1; i <= 10; i++)
          printf("%d ", i);
      printf("\n");
      printf("End of the main function...\n");
      return 0;
  }
  ```

---

### Question 3 [CO3] [Marks: 8 + 2 = 10]
- **a)** Find which CPU scheduling is more efficient between priority scheduling and Round Robin in terms of waiting time and context switch, where quantum time is 4 ($q = 4$). [8]

  | Process | Arrival time | Burst time | Priority |
  | :--- | :--- | :--- | :--- |
  | **P0** | 14 | 9 | 5 |
  | **P1** | 5 | 10 | 2 |
  | **P2** | 0 | 13 | 3 |
  | **P3** | 9 | 8 | 1 |
  | **P4** | 20 | 11 | 1 |

- **b)** What is starvation and how can it be solved? [2]

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets, headers/footers, footnotes, comments; list numbering, formatting and images are not reproduced -->

Department of Computer Science and Engineering

MIDTERM EXAMINATION Summer 2022

CSE321: Operating Systems

Total Marks: 30 Time Allowed: 1 Hour 15 min

[Answer All Questions. Understanding the question is a part of the exam.]

| CO1 / CO2 | 1. | a) What is multiprogramming? What are the requirements for multiprogramming? / b) Explain User mode and Kernel mode execution with example. / c) Define system call? Explain how system call works using System Call Interface. | 3 / 4 / 3 |

| CO2 | 2. | a) Though we can increase the hardware resources as much as we want by money, still why do we implement the thread? Explain how the thread accelerates the computing power with an example. / b) A system has processes to execute of which 40% is parallel. If the number of cores is increased from 2 to 4, what will be the increase in performance? / c) Find the output of the following code - / int main() / { / int id, i; / printf("Start of main...\n"); / id = fork(); / if (id > 0) { / printf("Parent section...\n"); / } / else if (id == 0) { / printf("\n fork created...\n"); / } / else { / printf("\n fork creation failed!!!\n"); / } / printf("Printing the numbers from 1 to 10\n"); / for (i = 1; i <= 10; i++) / printf("%d ", i); / printf("\n"); / printf("End of the main function...\n"); / return 0; / } | 3 / 3 / 4 |

| CO3 | 3. | a) Find which CPU scheduling is more efficient between priority scheduling and Round Robin in terms of waiting time and context switch. Where quantum time is 4 (q=4). / | Process | Arrival time | Burst time | Priority | / | P0 | 14 | 9 | 5 | / | P1 | 5 | 10 | 2 | / | P2 | 0 | 13 | 3 | / | P3 | 9 | 8 | 1 | / | P4 | 20 | 11 | 1 | / b) What is starvation and how can it be solved? | 8 / 2 |
