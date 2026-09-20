---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# Practice Problems on System Calls

Practice problems covering C Linux system calls: file I/O (`open`, `write`), process hierarchy with `fork()` and ordered printing (`wait`), odd PID conditional fork counting, `exec` with sort/oddeven programs, and process tree ID printing.
Docx last modified: 27-06-2026.

## Content

### Task 1: File Creation & Continuous String Input Loop
Write a C program that opens a file passed via command-line arguments and repeatedly prompts the user to enter strings to write into the file.
- The input loop continues until the user inputs `"-1"`.
- If the target file does not exist, the program must automatically create it (using `open()` flags `O_CREAT | O_WRONLY | O_TRUNC` / `O_APPEND`).

### Task 2: Ordered 3-Generation Process Hierarchy
Write a program creating a parent, child, and grandchild process hierarchy where each process outputs its identity:
- Parent: `"I am parent"`
- Child: `"I am child"`
- Grandchild: `"I am grandchild"`
- Use process synchronization (`wait()` / `waitpid()`) to enforce the exact reverse execution order:
  ```text
  I am grandchild
  I am child
  I am parent
  ```

### Task 3: Conditional Forking on Odd PID & Process Count
Given the initial code structure:
```c
a = fork();
b = fork();
c = fork();
```
Write a complete program that checks the PID of each newly created child process. If the child's PID is **odd**, that process must spawn another child process. Finally, count and print the total number of processes created (including the initial parent process).

### Task 4: Multi-Program Execution via Sort and Odd/Even Modules
1. Write a standalone program `sort.c` taking numbers via command-line arguments and printing them sorted in descending order.
2. Write a standalone program `oddeven.c` taking numbers via command-line arguments and printing whether each number is odd or even.
3. Write a controller program that forks a child process:
   - Child process sorts an array using `sort.c` (via `exec`).
   - Parent process evaluates and prints the odd/even status of each array element using `oddeven.c`.

### Task 5: 1 Child and 3 Grandchildren Process Tree
Write a C program where the parent process spawns 1 child process, and that child process spawns 3 grandchild processes. Each process prints its role and process ID.
- **Expected Output Format**:
  ```text
  1. Parent process ID : 0 (or getpid())
  2. Child process ID: ...
  3. Grand Child process ID: ...
  4. Grand Child process ID: ...
  5. Grand Child process ID: ...
  ```
