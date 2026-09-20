---
source: CSE321_pyq_sp23_midterm_setB.docx (the original, not included in this repo)
fidelity: the summary above the divider is paraphrased and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 1 embedded image(s); see the summary or the source)
source-mtime: 2025-01-31T20:04:50
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Midterm Examination Spring 2023 Set B

CSE321 Operating Systems Spring 2023 Set B Midterm exam covering dual-mode OS operation, layered vs microkernel structures, IPC across networked systems, fork() array mutation output tracing, Round Robin and SRTF CPU scheduling, Amdahl's Law speedup, and multithreading models (Many-to-One model analysis).
Docx last modified: 31-01-2025.

## Content

### Exam Metadata
- **Institution**: BRAC University, Department of Computer Science and Engineering
- **Course**: CSE 321: Operating Systems
- **Examination**: Midterm Examination (Set B)
- **Semester**: Spring 2023
- **Duration**: 1 Hour 15 Minutes
- **Total Marks**: 25

---

### Question 1 [CO1] [Marks: 2 + 3 + 2 + 3 = 10]
- **a)** Briefly explain dual mode operation of OS. [2]
- **b)** Distinguish between layered and microkernel OS structures. [3]
- **c)** Two processes (A and B) are distributed across multiple physical machines or networked systems. Which technique can be used to achieve inter-process communication in this scenario? Is it suitable for exchanging large amounts of data? Provide proper justification to support your answer. [2]
- **d)** Find the output of the following code snippet: [3]
  ```c
  const int len = 2;
  int main() {
      int id;
      int a[] = {5, 8};
      int b = len - 1;
      id = fork();
      if (id < 0) {
          printf("fork failed\n");
      }
      else if (id > 0) {
          wait(NULL);
          printf("parent process executing\n");
      }
      else {
          printf("child process executing\n");
          a[b - 1] = a[b - 1] + 2;
          a[b] = a[b] - 3;
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
- **a)** A system has processes to execute of which 45% is parallel. If the number of cores is increased from 2 to 4, Explain what will be the increase/decrease in performance. [2]
- **b)** Describe the multithreading model which is shown in the picture below (Diagram shows 4 User Threads $T$ mapped to 1 Kernel Thread $K$, i.e. the Many-to-One multithreading model). Identify the issue in this model and suggest another multithreading model which is free from that issue. [3]

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets, headers/footers, footnotes, comments; list numbering, formatting and images are not reproduced -->

Department of Computer Science and Engineering

  (text box) B

Midterm Examination Spring 2023

CSE 321: Operating Systems

| Duration: 1 Hour 15 Minutes | Total Marks: 25 |

Answer the following questions.

Figures in the right margin indicate marks.

| 1. / CO1 | a) Briefly explain dual mode operation of OS. / b) Distinguish between layered and microkernel OS structures. / c) Two processes (A and B) are distributed across multiple physical machines or networked systems. Which technique can be used to achieve inter-process communication in this scenario? Is it suitable for exchanging large amounts of data? Provide proper justification to support your answer. / d) Find the output of the following code snippet. / | const int len = 2; / int main(){ / int id; / int a[] = {5,8}; / int b = len-1; / id = fork(); / if (id < 0){ / printf("fork failed\n"); / } / else if(id > 0){ / wait(NULL); / printf("parent process executing\n"); / } / else{ / printf("child process executing\n"); / a[b-1]=a[b-1]+2; / a[b] = a[b]-3; / } / for(int i=0;i<len;i++){ / printf("value of a[%d]: %d\n",i,a[i]); / } / return 0; / } | | [2] / [3] / [2] / [3] |

| 2. / CO2 | | Processes | Arrival Time | Burst Time | / | P1 | 0 | 11 | / | P2 | 20 | 6 | / | P3 | 14 | 9 | / | P4 | 20 | 8 | / | P5 | 15 | 8 | / | P6 | 16 | 8 | / | P7 | 2 | 2 | / a) Draw a Gantt chart and illustrate the execution of the process using the Round Robin scheduling algorithm (time quantum = 5 units). Calculate the average waiting and turnaround time, | [3+2] |

|  | b) Apply Shortest Remaining Time First (SRTF) scheduling algorithm. Draw the Gantt chart and Calculate the average waiting and turnaround time. / c) Compare the results and identify the most suitable scheduling algorithm in this scenario. | [2+2] / [1] |

| 3. / CO3 | a) A system has processes to execute of which 45% is parallel. If the number of cores is increased from 2 to 4, Explain what will be the increase/decrease in performance. / b) Describe the multithreading model which is shown in the picture below. Identify the issue in this model and suggest another multithreading model which is free from that issue. | [2] / [3] |
