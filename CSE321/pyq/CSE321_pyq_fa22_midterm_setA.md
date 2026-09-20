---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# CSE 321 Midterm Examination Fall 2022 (Set A)

Midterm examination question paper for CSE321 Fall 2022 Set A covering time-sharing systems, system call interface, fork() output prediction, Round Robin, and Preemptive Priority scheduling.
Docx last modified: 31-01-2025.

## Content

**Course**: CSE 321: Operating Systems  
**Semester**: Fall 2022  
**Exam**: Midterm Examination [Set A]  
**Duration**: 1 Hour 10 Minutes | **Total Marks**: 25  

---

### Question 1 [CO1] (Total: 5 Marks)
- **a)** [3 Marks] State time-sharing system with an example.
- **b)** [2 Marks] Mention the roles of the system call interface.

---

### Question 2 [CO2] (Total: 7 Marks)
- **a)** [3 Marks] Distinguish between CPU scheduler and job scheduler.
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
        printf("child finished
");
    }
    else{
        wait(NULL);
        printf("parent started
");
        x = x - 2;
        y = y + 5;
        printf("values of x: %d & y: %d
", x, y);
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
- **a)** [2 Marks] When is CPU scheduling required?
- **b)** [3 + 2 + 1 Marks] Draw a Gantt chart and illustrate the execution of the process using the **Round Robin** scheduling algorithm (time quantum = 11 units). Calculate the average waiting time and number of context switches.

| Processes | Arrival Time | Burst Time |
| :--- | :--- | :--- |
| P1 | 3 | 37 |
| P2 | 12 | 17 |
| P3 | 58 | 28 |
| P4 | 59 | 21 |
| P5 | 68 | 19 |

- **c)** [3 + 2 Marks] Consider the following set of processes with the length of the CPU-burst time given in milliseconds. Draw the Gantt Charts illustrating the execution of these processes using **preemptive priority** (the lowest number implies a higher priority). Calculate the average turnaround time for the below data set.

| Processes | Priority | Arrival Time | Burst Time |
| :--- | :--- | :--- | :--- |
| P1 | 12 | 0 | 4 |
| P2 | 8 | 1 | 2 |
| P3 | 6 | 2 | 3 |
| P4 | 2 | 3 | 5 |
| P5 | 4 | 4 | 1 |
| P6 | 1 | 5 | 4 |
| P7 | 3 | 6 | 6 |\n