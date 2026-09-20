---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# Practice Problems on System Calls (Lab 01)

Lab 01 practice problems on C system calls: file I/O loops, 3-level process hierarchies and synchronization, conditional forking based on PID parity, sorting/parity multi-program execution, and process ID tree tracking.
Docx last modified: 30-06-2026.

## Content

### Task 1: File Creation and Continuous String Input
Write a C program that opens a file passed via command-line arguments and continuously takes user input strings to write to the file until `"-1"` is entered.
- Automatically create the file if it does not already exist.

### Task 2: Synchronized 3-Level Process Hierarchy Output
Write a program creating a parent, child, and grandchild process where each prints its identity.
- Parent: `"I am parent"`
- Child: `"I am child"`
- Grandchild: `"I am grandchild"`
- Enforce output order using synchronization:
  ```text
  I am grandchild
  I am child
  I am parent
  ```

### Task 3: Odd PID Conditional Forking and Process Counting
Given:
```c
a = fork();
b = fork();
c = fork();
```
Check each child process's PID; if it is odd, the process must create another child process. Print the total count of processes created starting from the root parent.

### Task 4: Modular Sort & Odd/Even Programs with Process Execution
- Create `sort.c`: takes CLI arguments and prints numbers sorted in descending order.
- Create `oddeven.c`: takes CLI arguments and prints odd/even classification for each number.
- Controller program: forks a child to sort the declared array via `sort`, then parent prints odd/even status via `oddeven`.

### Task 5: 1 Child and 3 Grandchildren Process Tree ID Display
Write a C program where the parent process creates 1 child process, and that child process creates 3 grandchild processes, printing all IDs in sequence:
```text
1. Parent process ID : ...
2. Child process ID: ...
3. Grand Child process ID: ...
4. Grand Child process ID: ...
5. Grand Child process ID: ...
```
