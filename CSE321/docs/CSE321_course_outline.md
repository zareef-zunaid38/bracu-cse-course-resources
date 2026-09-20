---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# CSE 321: Operating Systems Course Outline

Course outline for CSE321 Operating Systems at BRAC University (Summer 2026), covering course outcomes, lesson plans, lab plans, grading, textbook, and policy details.
Docx last modified: 07-06-2026.

## Content

### Course Description & Prerequisites
- **Course**: CSE 321: Operating Systems (Semester: Summer 2026)
- **Description**: General understanding of principles and concepts governing operating system functions and state-of-the-art approaches making design, implementation, and operation of complex OS possible. Core topics: concurrent programming (threads and synchronization), inter-process communication (IPC), process and thread management, storage and resource management/communication, and security/protection analysis.
- **Prerequisites**: CSE221: Algorithms
- **Co-requisites**: None

### Course Outcomes (COs) & CO-PO-Bloom Mapping
- **CO1**: Understand concepts of processes and their management, CPU scheduling techniques; Apply system calls for solving problems on process management; Analyze security concerns and protection mechanisms of OS. (PO1, Cognitive / Apply, Analyze. Assessment: Midterm Exam, Final Exam, Lab Work).
- **CO2**: Understand behaviors of threads and their management, process and thread synchronization mechanisms, deadlocks, virtual machines and containers; Apply thread libraries for solving problems on thread management and synchronization; Understand mechanisms of file systems concepts and their implementations in OS, and Apply concepts to design complex file systems. (PO2, Cognitive / Apply, Analyze. Assessment: Midterm Exam, Final Exam, Lab Work).
- **CO3**: Understand mechanisms of main memory and virtual memory management in OS. (PO5, Cognitive / Understand, Apply, Analyze. Assessment: Final Exam, Lab Work).

### Lesson Plan (Theory Topics by Week)
1. **Week 1, 2, 3 (CO1)**: Process concepts, states, Inter Process Communication (IPC), operation on the process (creation and termination).
2. **Week 3, 4 (CO2)**: Threads overview, Multicore Programming, Multithreading Models, Thread Libraries, Threading Issues, and operating systems examples.
3. **Week 4, 5 (CO1)**: Basic concepts of CPU scheduling, scheduling criteria, scheduling algorithms and simulations (FCFS, SJF, Priority, Round Robin), Multilevel queue and Multilevel feedback queue algorithms.
4. **Week 5, 6, 7 (CO2)**: Concepts of Process Synchronization, Race Condition, Critical Section Problem, Mechanisms of Process Synchronization: Test and Set, Compare and Swap, Mutex locks, Semaphores, Classical synchronization problems.
5. **Week 9, 10, 11 (CO2)**: Magnetic Disk Structure Overview, Magnetic Disk Architecture, File Concepts, File Attributes, File Operations, Indexed File Allocation Method, UNIX Inode, File System Implementation, Crash consistency and journaling.
6. **Week 10, 11, 12 (CO3)**: Main Memory Management Background, Address spaces, paging, implementation of page table, shared paging, hierarchical page table. Introduction of virtual memory, demand paging, performance of demand paging, page fault handling, page replacement algorithms (FIFO, LRU, Optimal).
7. **Week 12, 13 (CO1)**: Security Problem, security violation categories and methods, security measure levels, program threats, implementing security defenses, firewall, goals and principles of protection, domain of protection, domain structure, domain implementation in unix and android, access matrix, revocation of access rights, mandatory access control, capability-based systems, other protection improvement methods and language-based protection.
8. **Week 13, 14 (CO2)**: Concepts of Virtual Machines and Containers and differences between them, Concepts of Hypervisors, Mobile OS concepts.

### Lab Plan (Schedule & Topics)
- **Class 0 (Weeks 1-2, 9 June - 18 June)**: Basic Shell Commands, Basic C Programming (buX online tutorials).
- **Class 1 (Week 3, 20 June - 25 June)**: System Calls in C (process manipulation, file descriptors, performance profiling, tracing).
- **Class 2 (Week 4, 27 June - 2 July)**: Linux Shell Scripting (automation, user input, CLI arguments).
- **Class 3 (Week 5, 4 July - 9 July)**: Assessment 1 (Basic shell commands & C programming).
- **Class 4 (Week 6, 11 July - 16 July)**: Threads and Synchronization (pthreads, mutex, semaphore, race condition solutions).
- **Class 5 (Week 7, 18 July - 23 July)**: IPC (pipes, shared memory, message passing).
- **Weeks 8, 9 (25 July - 6 Aug)**: Mid-Week — No Lab.
- **Class 6 (Week 10, 8 Aug - 13 Aug)**: xv6 Introduction (installation tutorial, booting process, kernel vs user space, file system hierarchy, creating user programs).
- **Class 7 (Week 11, 15 Aug - 20 Aug)**: xv6 Features (system call implementation & tracing, process structure, context switching, scheduler control, preemptive scheduling).
- **Class 8 (Week 12, 22 Aug - 27 Aug)**: Assessment 2 (System calls, threads and synchronization).
- **Class 9 (Week 13, 29 Aug - 3 Sep)**: Review & Project Viva.

### Course Assessment Methods & Weights
- **Assignments**: 5%
- **Quizzes / Class Tests**: 10%
- **Midterm Examination**: 25%
- **Lab & Projects**: 25%
- **Final Examination**: 35%
- **Total**: 100%

### CO-Wise Tentative Marks Distribution
- **CO1**: 30%
- **CO2**: 35%
- **CO3**: 35%

### Textbooks
1. *Operating System Concepts Essentials* by Abraham Silberschatz, Peter B. Galvin, Greg Gagne, Wiley; 10th Edition (2008/2018), ISBN: 978-1119320913.
2. *Modern Operating Systems* by Andrew S. Tanenbaum, Prentice Hall; 4th Edition (2015), ISBN-13: 978-0133591620.
3. *Operating Systems: Three Easy Pieces* by Remzi H. Arpaci-Dusseau and Andrea C. Arpaci-Dusseau, Arpaci-Dusseau Books, Version 1.10 (October 2023), http://www.ostep.org.

### Course Policies
- **Attendance**: Mandatory for classroom and lab. Over 30% absence bars student from final exam.
- **Honor Code**: Cheating, plagiarism, or academic dishonesty results in an "F" grade.
- **Late Work / Exams**: Late assignments not accepted. Closed-book exams. No make-ups for quizzes or midterm except for documented hospitalization or detention.
- **Grading Scale**:
  - 90–100: A (4.0)
  - 85–<90: A- (3.7)
  - 80–<85: B+ (3.3)
  - 75–<80: B (3.0)
  - 70–<75: B- (2.7)
  - 65–<70: C+ (2.3)
  - 60–<65: C (2.0)
  - 57–<60: C- (1.7)
  - 55–<57: D+ (1.3)
  - 52–<55: D (1.0)
  - 50–<52: D- (0.7)
  - <50: F (0.0)
