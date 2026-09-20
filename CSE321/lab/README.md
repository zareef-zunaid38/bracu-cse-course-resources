# CSE321 — Lab guide

Lab faculty: MAFZ / SZD. Back to the [course guide](../README.md). All of this is faculty-issued lab material; the [lab outline](cse321_lab_outline.md) has the schedule, marks and policies, so read it first.

## What the lab is made of

| Part | Where |
|---|---|
| Lab outline: schedule, assessments, assignments, project, attendance | [`cse321_lab_outline.md`](cse321_lab_outline.md) |
| Handouts, example code and practice, labs 0.1 to 6 | [`lab-tasks/`](lab-tasks) |
| The two lab assignment prompts | [`lab-assignments/`](lab-assignments) |
| Term project: SimpleFS (a small file system in C) | [`project/`](project) |

## The labs in order

The first assessment is on basic shell commands and C, which are **self-taught** from the intro handouts (the videos are on BUX), so start on labs 0.1 to 0.3 before the semester gets busy.

| Lab | Topic | What is here |
|---|---|---|
| 0.1 | Linux shell commands | [handout](<lab-tasks/Lab 0.1_Linux_Shell_Commands.pdf>), [practice problems](lab-tasks/practice_problems_shell.md) |
| 0.2, 0.3 | Introduction to C programming | handouts [0.2](<lab-tasks/Lab 0.2_Introducing_C_Programming.pdf>) and [0.3](<lab-tasks/Lab 0.3_Introducing_C_Programming.pdf>), [practice problems](lab-tasks/practice_problems_c.md) |
| 1 | System calls and process creation | [`lab01/`](lab-tasks/lab01): lecture, [practice problems](<lab-tasks/lab01/Practice_System Calls.md>); more in [system calls practice](lab-tasks/practice_problems_system_calls.md) |
| 2 | Linux shell scripts | [`lab02/`](lab-tasks/lab02): lecture, [bash scripting practice](<lab-tasks/lab02/Practice_Bash Scripting.md>) |
| 3 | Threads and synchronization | [`lab03/`](lab-tasks/lab03): pthreads examples (`01_Threads`), mutex and semaphore examples (`02_Synchronization`) |
| 4 | Inter-process communication | [`lab04/`](lab-tasks/lab04): pipes, shared memory, message passing, key generation, and [IPC practice problems](<lab-tasks/lab04/Practice Problems/Practice_IPC.md>) |
| 5 | Introduction to xv6 | [`lab05/`](lab-tasks/lab05): setup guides (lab PCs and Apple silicon), version control in xv6, files and components, user programs, system calls |
| 6 | xv6 process scheduling | [`lab06/`](lab-tasks/lab06): how the scheduler works, the priority-scheduler exercise with its patch and a demo video, and lottery-scheduling reading |

## Assignments and project

- **Assignment 1** — [threads and synchronization](lab-assignments/CSE321_lab_assignment1.pdf) (multithreaded Fibonacci, and a sandwich-maker synchronization problem).
- **Assignment 2** — [xv6 lottery scheduler](lab-assignments/CSE321_lab_assignment2.pdf): replace xv6's round-robin scheduler. Labs 5 and 6 are its groundwork.
- **Term project** — [SimpleFS specification](<project/[Summer 2026] CSE321 Lab Project Specifications.pdf>): two C programs that build and fill a small file-system image. There is also a supplementary [animated walkthrough](project/CSE321_SimpleFS_Animated_Project_Walkthrough.html) (an HTML file, so download it and open it in a browser — the spec is authoritative if they differ) and a [printout of the submission form's instructions](<project/[Summer 2026] CSE321 Project Submission.pdf>).

## My xv6 work

[zareef-zunaid38/xv6-riscv](https://github.com/zareef-zunaid38/xv6-riscv) is a fork of MIT's xv6-riscv where I worked through the priority-scheduler exercise from labs 5 and 6. Treat it as a reference for how that exercise can be approached, not as an answer key.
