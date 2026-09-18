# CSE 321 Midterm Examination Fall 2022 (Set B) — sidecar (read this, not the .docx)

Midterm examination question paper for CSE321 Fall 2022 Set B covering dual-mode operation, OS services, process states, fork() code tracing, Multilevel Queue starvation, Round Robin, and Preemptive Priority scheduling.
Docx last modified: 31-01-2025.

## Content

**Course**: CSE 321: Operating Systems  
**Semester**: Fall 2022  
**Exam**: Midterm Examination [Set B]  
**Duration**: 1 Hour | **Total Marks**: 25  

---

### Question 1 [CO1] (Total: 5 Marks)
- **a)** [3 Marks] Explain dual-mode operation.
- **b)** [2 Marks] Briefly explain any two services of the OS.

---

### Question 2 [CO2] (Total: 7 Marks)
- **a)** [3 Marks] Explain each process state with an example.
- **b)** [4 Marks] Find the output of the following code snippet:
```c
int main(){
    int id;
    static int x = 10;
    int y = 5;
    id = fork();
    if (id < 0){
        printf("fork failed
");
    }
    else if(id == 0){
        printf("child started
");
        x = x + 5;
        y = y - 3;
        printf("values of x: %d & y: %d
", x, y);
        printf("child finished
");
    }
    else{
        wait(NULL);
        printf("parent started
");
        printf("parent finished
");
    }
    x = x + 5;
    y = y - 5;
    printf("values of x: %d & y: %d
", x, y);
    printf("terminating
");
    return 0;
}
```

---

### Question 3 [CO3] (Total: 13 Marks)
- **a)** [2 + 6 Marks]
  1. “Multilevel-queue can prevent starvation problem” — Justify your answer.
  2. Draw a Gantt chart and illustrate the execution of the process using the **Round Robin** scheduling algorithm (time quantum = 12 units). Calculate the average waiting time and number of context switches.

| Processes | Arrival Time | Burst Time |
| :--- | :--- | :--- |
| P1 | 3 | 37 |
| P2 | 12 | 17 |
| P3 | 62 | 28 |
| P4 | 63 | 21 |
| P5 | 72 | 19 |

- **b)** [3 + 2 Marks] Consider the following set of processes with the length of the CPU-burst time given in milliseconds. Draw the Gantt Charts illustrating the execution of these processes using **preemptive priority** (the highest number implies a higher priority). Calculate the average turnaround time for the below data set.

| Processes | Priority | Arrival Time | Burst Time |
| :--- | :--- | :--- | :--- |
| P1 | 2 | 0 | 4 |
| P2 | 4 | 1 | 2 |
| P3 | 6 | 2 | 3 |
| P4 | 10 | 3 | 5 |
| P5 | 8 | 4 | 1 |
| P6 | 12 | 5 | 4 |
| P7 | 9 | 6 | 6 |\n