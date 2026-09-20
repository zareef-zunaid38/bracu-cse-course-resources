---
source: CSE321_theory_module01_process.pptx (the original, included in this repo)
fidelity: the summary above the divider was rewritten from the deck on 2026-09-20 after an audit found errors in the earlier version; it is still a condensed paraphrase and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 14 embedded image(s); informational images are described in the summary)
source-mtime: 2025-10-11T21:50:24
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Module 01: Process Management

Lecture slides (38) on processes: the process concept and its memory layout, states, the PCB, scheduling queues and context switches, dual-mode operation, system calls, process creation/termination with `fork()`, and inter-process communication. Sections in the deck: Process (1–10), Process Scheduling (11–20), Operations on Process (21–31), Interprocess Communication (32–38).
Pptx last modified: 11-10-2025.
Rewritten 2026-09-20 directly from the deck (an audit found the earlier summary's slide numbers and sections unreliable). Diagram and code content below was read from renders of slides 3, 7, 10, 12–14, 16, 18, 25, 26, 34, 36 and 37.

## Content

### Slides 2–10: The process
- **Slide 2, process concept:** what to call CPU activities — jobs (batch systems), user programs/tasks (time-sharing); all are called *processes*, and "job" and "process" are used almost interchangeably. Speaker notes: a single-user system may run several programs at once (word processor, browser, e-mail), and even without multitasking the OS has its own internal activities; the text prefers "process" although "job" dates from when job processing was the main OS activity.
- **Slide 3:** a process is a program in execution, more than its code (the *text section*). It also holds the program counter and registers (current activity), the stack (function parameters, local variables, return addresses), the data section (global variables) and the heap (memory allocated at run time). Diagram: memory from 0 upward — text, data, heap, then free space, then stack at max.
- **Slide 4, program vs process:** a program is a passive entity on disk (executable file); a process is active. A program becomes a process when an executable is loaded into memory (started by a GUI click, typing its name, etc.). One program can be several processes (several users running the same program). Example: `a=10; b=5; print(a+b)` in `program1.exe` → loaded into memory as text section, data section (`a` = 10, `b` = 5) and a program counter holding the next address.
- **Slide 5:** same program, different processes — one browser program with three windows = processes 1–3; code is the same, data/heap/stack differ.
- **Slide 6, states:** New (being created), Running (instructions executing), Waiting (for an event), Ready (waiting for a processor), Terminated (finished). **Slide 7 diagram:** new →(admitted)→ ready; ready →(scheduler dispatch)→ running; running →(interrupt)→ ready; running →(I/O or event wait)→ waiting; waiting →(I/O or event completion)→ ready; running →(exit)→ terminated.
- **Slide 8, PCB (Process/Task Control Block)** stores: process state, program counter, CPU registers, CPU-scheduling information (priorities, queue pointers), memory-management information, accounting information (CPU used, clock time since start, time limits), I/O status (allocated devices, open files).
- **Slide 9, threads:** so far one thread of execution per process; multiple program counters per process give multiple threads of control, so the PCB must hold thread details (covered later).
- **Slide 10, Linux `task_struct`:** `pid_t pid; long state; unsigned int time_slice; struct task_struct *parent; struct list_head children; struct files_struct *files; struct mm_struct *mm;` (process id, state, scheduling info, parent, children, open files, address space). Diagram: a linked list of task_struct entries with `current` pointing at the running process.

### Slides 11–20: Process scheduling, context switch, dual mode, system calls
- **Slide 12:** the process scheduler picks among available processes for the next CPU core; goal: maximize CPU use and switch processes quickly; it keeps scheduling queues — the *ready queue* (processes in main memory ready and waiting) and *wait queues* (processes waiting for an event such as I/O); processes migrate among queues. Diagram: a ready queue and a wait queue drawn as head/tail pointers linking PCBs.
- **Slide 13, queueing diagram:** ready queue → CPU; from the CPU a process can go to an I/O request → I/O wait queue → back to ready; or time slice expired → ready; or create child process → wait for child termination → ready; or wait for an interrupt → interrupt wait queue → ready.
- **Slides 14–15, context switch:** happens when the CPU switches from one process to another: (1) store the current process's context, (2) restore the next process's context. Context is held in the PCB. It is pure overhead — no useful work is done while switching; the more complex the OS and the PCB, the longer it takes; the time depends on hardware support (some hardware has multiple register sets per CPU, so several contexts load at once). Diagram: P0 executing → interrupt/system call → OS saves state into PCB0, reloads state from PCB1 → P1 executes (idle gaps marked as overhead), and the reverse switch back.
- **Slide 16, dual-mode operation:** the OS must distinguish OS code from user code; a *mode bit* in hardware marks kernel (0) or user (1) mode; dual mode protects the OS from errant users by making harmful instructions *privileged* (executable only in kernel mode). Diagram: user process executing → calls system call (trap, mode bit = 0) → kernel executes the system call → return from system call (mode bit = 1).
- **Slide 17, system calls:** the programming interface to OS services; typically written in C/C++; usually reached through a high-level API rather than directly. Three common APIs: Win32 (Windows), POSIX (UNIX, Linux, Mac OS X), Java API (for the JVM). The call names in the text are generic.
- **Slide 18, system-call interface:** each call has a number; the interface keeps a table indexed by it, invokes the intended call in the kernel and returns status and values; the caller only needs to obey the API — details are hidden by the run-time support library. Figure: user application calls `open()` → system-call interface (table indexed by number) → implementation of `open()` in kernel mode → return.
- **Slides 19–20, types of system call** (Windows / Linux): process control — `CreateProcess()`/`ExitProcess()`/`WaitForSingleObject()` / `fork()`/`exit()`/`wait()`; file manipulation — `CreateFile()`/`ReadFile()`/`WriteFile()`/`CloseHandle()` / `open()`/`read()`/`write()`/`close()`; device manipulation — `SetConsoleMode()`/`ReadConsole()`/`WriteConsole()` / `ioctl()`/`read()`/`write()`; information maintenance — `GetCurrentProcessID()`/`SetTimer()`/`Sleep()` / `getpid()`/`alarm()`/`sleep()`; communication — `CreatePipe()`/`CreateFileMapping()`/`MapViewOfFile()` / `pipe()`/`shm_open()`/`mmap()`; protection — `SetFileSecurity()`/`InitlializeSecurityDescriptor()`/`SetSecurityDescriptorGroup()` / `chmod()`/`umask()`/`chown()`.

### Slides 21–31: Operations on processes
- **Slides 22–24, creation:** the system must provide process creation and termination. A parent creates children, forming a tree of processes identified by a pid (slide 23 example tree: P1 pid 1, P2 pid 8, P3 pid 58, P4 pid 63, P5 pid 51, P6 pid 38, P7 pid 55). Resource sharing options: parent and children share all resources, children share a subset, or none. Execution options: parent and children run concurrently, or the parent waits until children terminate. Address-space options: the child is a duplicate of the parent, or a new program is loaded into it.
- **Slide 25, UNIX:** `fork()` creates a new process that becomes the child of the caller; `exec()` runs an executable file, replacing the previous one; `wait()` suspends the caller until one of its children terminates. Diagram: parent → `pid = fork()`; child (pid = 0) → `exec()` → `exit()`; parent (pid > 0) → `wait()` → parent resumes.
- **Slide 26 (Figure 3.9, code):**
  ```c
  #include <sys/types.h>
  #include <stdio.h>
  #include <unistd.h>
  int main()
  {
      pid_t pid;
      /* fork a child process */
      pid = fork();
      if (pid < 0) { /* error occurred */
          fprintf(stderr, "Fork Failed");
          return 1;
      }
      else if (pid == 0) { /* child process */
          execlp("/bin/ls","ls",NULL);
      }
      else { /* parent process */
          /* parent will wait for the child to complete */
          wait(NULL);
          printf("Child Complete");
      }
      return 0;
  }
  ```
- **Slides 27–30, exercises (code only, no answers on the slides):** slide 27 — `main(){ fork(); fork(); printf("A"); }` and the same with three `fork()`s; slide 28 — `a = fork(); if(a==0) fork(); fork(); printf("A");` and `fork(); a = fork(); if(a==0) fork(); printf("A");`; slide 29 — `x = 1; a = fork(); if (a == 0) { x = x - 1; print("value of x is:", x) } else if (a > 0) { wait(NULL); x = x + 1; print("value of x is:", x) }`; slide 30 — `x = 1; a = fork(); if (a == 0) { print("Child"); x = x - 1 } else if (a > 0) { print("Parent"); x = x + 1; wait(NULL); x = x + 1 } print("value of x is:", x)`.
- **Slide 31, termination:** a process ends by calling `exit()` (returns status to the parent via `wait()`, and the OS deallocates its resources). A parent may end a child with `abort()` when the child exceeds its resources, its task is no longer needed, or the parent is exiting and the OS does not allow orphaned children. Some OSs enforce *cascading termination* (children, grandchildren, … are all terminated, initiated by the OS). The parent waits with `pid = wait(&status);`. A process whose parent never called `wait()` is a **zombie**; if the parent terminated without calling `wait()`, the process is an **orphan**.

### Slides 32–38: Interprocess communication
- **Slide 33:** concurrent processes are independent (cannot affect or be affected by others) or cooperating (can); cooperation is needed for information sharing, computational speedup, modularity and convenience.
- **Slide 34, IPC** is a mechanism to exchange data among processes; two models — shared memory and message passing. Diagram: message passing (processes A and B exchange through a message queue m1…mn in the kernel) vs shared memory (A and B both map a shared region, with the kernel outside).
- **Slides 35–37, shared memory / producer–consumer:** a producer creates items for a consumer through a shared buffer. Variables `in` (next free position) and `out` (first full position), both initialised to 0; here `BUFFER_SIZE = 7` (circular buffer). Buffer full: `in = 6, out = 0` in the slide's example (slots [0]–[5] filled, [6] empty); not full: `in = 4, out = 0`; consumer side — empty: `in = 0, out = 0`; not empty: `in = 5, out = 0`. Producer code (slide 36): `item next_produced; while (true) { /* produce an item in next_produced */ while (((in + 1) % BUFFER_SIZE) == out) ; /* do nothing */ buffer[in] = next_produced; in = (in + 1) % BUFFER_SIZE; }`. Consumer code (slide 37): `item next_consumed; while (true) { while (in == out) ; /* do nothing */ next_consumed = buffer[out]; out = (out + 1) % BUFFER_SIZE; /* consume the item in next_consumed */ }`.
- **Slide 38, message passing:** processes P and Q communicate by `send(message)` and `receive(message)` over a communication link; useful for small amounts of data and better suited to distributed systems than shared memory.

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets; list numbering, formatting and images are not reproduced -->

<!-- slide 1 -->
### Operating Systems Process
FBA

<!-- slide 2 -->
### Process Concept
What to call the activities of CPU ?
Jobs
User Programs
or
Tasks
Batch System
Time Sharing System
These activities are called “Processes”
The terms “job” and “process” are used almost interchangeably.
Notes: A question that arises in discussing operating systems involves what to call all the CPU activities. A batch system executes jobs, whereas a time-shared system has user programs, or tasks. A single-user system, a user may be able to run several programs at one time: a word processor, aWeb browser, and an e-mail package. Even if a device or system does not support multitasking, the operating system may need to support its own internal programmed activities. In many respects, all these activities are similar, so we call all of them processes. Although we personally prefer the term process, much of operating-system theory and terminology was developed during a time when the major activity of operating systems was job processing.

<!-- slide 3 -->
### Process
[image]
A process is a program that is in execution.
But, it is more than the program codes. Program code is known as “text section” of a process.
Besides code of the program, it contains -
Program Counter and Registers: stores current activity of the process
Stack: Temporary data (function parameter, local variables, return addresses etc.)
Data Section: Global Variables
Heap: dynamically allocated memory during runtime

<!-- slide 4 -->
### Program Vs Process
Program is passive entity stored on disk (executable file); process is active
  Program becomes process when an executable file is loaded into memory
Execution of program started via GUI mouse clicks, command line entry of its name, etc.
One program can be several processes
  Consider multiple users executing the same program
a=10
b=5
print(a+b)
program1.exe
Program
Loaded into
memory
Value of a is 10
print(a+b)
b=5
a=10
Text section
Data section
next address to execute
Program counter
…
...
Corresponding process in memory

<!-- slide 5 -->
### Same program, Different Process
Code 1
Browser program
Process 1
Process 2
Process 3
Browser ( first window)
Browser ( second window)
Browser ( third window)
Program code is same
Data, Heap, Stacks contains different information

<!-- slide 6 -->
### States of a Process
A process state defines the current activity of that process.
The states a process can be:
New: Process is being created
Running: Instructions are being executed
Waiting: Process is waiting for some event to occur
Ready: Waiting to be assigned to a processor
Terminated: Process has finished execution

<!-- slide 7 -->
### Process State Diagram
[image]

<!-- slide 8 -->
### Representation of Processes in OS
Each process is represented in the operating system by a Process Control Block (PCB) or Task Control Block
PCB is a data structure to store information of Processes such as -
[image]
Process state – running, waiting, etc.
Program counter – location of instruction to next execute
CPU registers – contents of all process-centric registers
CPU scheduling information- priorities, scheduling queue pointers
Memory-management information – memory allocated to the process
Accounting information – CPU used, clock time elapsed since start, time limits
I/O status information – I/O devices allocated to process, list of open files

<!-- slide 9 -->
### Threads
So far, process has a single thread of execution
Consider having multiple program counters per process
  Multiple locations can execute at once
    Multiple threads of control -> threads
Must then have storage for thread details, multiple program counters in PCB
Explore the details later

<!-- slide 10 -->
### Process Representation in Linux
Represented by the C structure task_struct
pid t_pid; /* process identifier */
long state; /* state of the process */
unsigned int time_slice /* scheduling information */
struct task_struct *parent; /* this process’s parent */
struct list_head children; /* this process’s children */
struct files_struct *files; /* list of open files */
struct mm_struct *mm; /* address space of this process */
[image]

<!-- slide 11 -->
### Operating Systems Process Scheduling
FBA

<!-- slide 12 -->
### Process Scheduling
Process scheduler selects among available processes for next execution on CPU core
Goal -- Maximize CPU use, quickly switch processes onto CPU core
Maintains scheduling queues of processes
  Ready queue – set of all processes residing in main memory, ready and waiting to execute
  Wait queues – set of processes waiting for an event (i.e., I/O)
  Processes migrate among the various queues
[image]

<!-- slide 13 -->
### Representation of Process Scheduling
[image]

<!-- slide 14 -->
### Context Switch
A context switch occurs when the CPU switches from one process to another.
Context Switch:
1. Storing currently executed process context
2. Restoring the next process context to execute
[image]
Overhead for context switch from P0 to P1
Overhead for context switch from P1 to P0

<!-- slide 15 -->
### Context Switch
When CPU switches to another process, the system must save the state of the old process and load the saved state for the new process via a context switch
Context of a process represented in the PCB
Context-switch time is pure overhead; the system does no useful work while switching
  The more complex the OS and the PCB => the longer the context switch
Time dependent on hardware support
  Some hardware provides multiple sets of registers per CPU => multiple contexts loaded at once

<!-- slide 16 -->
### Dual Mode Operation
Need to distinguish between the execution of operating-system code and user defined code.
A bit, called the mode bit, is added to the hardware of the computer to indicate the current mode: kernel (0) or user (1).
dual mode of operation provides protection of the operating system from errant users
this protection is provided by designating some of the machine instructions that may cause harm as privileged instructions that are executed only in kernel mode.
[image]

<!-- slide 17 -->
### System Calls
Programming interface to the services provided by the OS
Typically written in a high-level language (C or C++)
Mostly accessed by programs via a high-level Application Programming Interface (API) rather than direct system call use
Three most common APIs are Win32 API for Windows, POSIX API for POSIX-based systems (including virtually all versions of UNIX, Linux, and Mac OS X), and Java API for the Java virtual machine (JVM)
Note: the system-call names used throughout this text are generic

<!-- slide 18 -->
### System Call Interface
Typically, a number is associated with each system call
  System-call interface maintains a table indexed according to these numbers
The system call interface invokes the intended system call in OS kernel and returns status of the system call and any return values
The caller need know nothing about how the system call is implemented
  Just needs to obey API and understand what OS will do as a result call
  Most details of OS interface hidden from programmer by API
    Managed by run-time support library (set of functions built into libraries included with compiler)
[image]
Figure: API – System Call – OS Relationship

<!-- slide 19 -->
### Types of System Call
| Type | Windows OS | Linux OS |
| Process Control | CreateProcess() / ExitProcess() / WaitForSingleObject() | fork() / exit() / wait() |
| File Manipulation | CreateFile() / ReadFile() / WriteFile() / CloseHandle() | open() / read() / write() / close() |
| Device Manipulation | SetConsoleMode() / ReadConsole() / WriteConsole() | ioctl() / read() / write() |

<!-- slide 20 -->
### Types of System Call
| Type | Windows OS | Linux OS |
| Information Maintenance | GetCurrentProcessID() / SetTimer() / Sleep() | getpid() / alarm() / sleep() |
| Communication | CreatePipe() / CreateFileMapping() / MapViewOfFile() | pipe() / shm_open() / mmap() |
| Protection | SetFileSecurity() InitlializeSecurityDescriptor() / SetSecurityDescriptorGroup() | chmod() / umask() / chown() |

<!-- slide 21 -->
### Operating Systems Operations on Process
FBA

<!-- slide 22 -->
### Operations on Processes
System must provide mechanisms for:
Process creation
Process termination

<!-- slide 23 -->
### Process Creation
Parent process create children processes, which, in turn create other processes, forming a tree of processes
Generally, process identified and managed via a process identifier (pid)
P1
pid = 1
P2
pid = 8
P3
pid = 58
P4
pid = 63
P5
pid = 51
P6
pid = 38
P7
pid = 55
Parent process
Child process
Resource sharing options
  Parent and children share all resources
  Children share subset of parent’s resources
  Parent and child share no resources
Execution options
  Parent and children execute concurrently
  Parent waits until children terminate

<!-- slide 24 -->
### Process Creation
When a process creates new process -
The parent continues to execute concurrently with its children
Or,
The parent waits until some or all of its children have terminated
Two address-space possibilities for the new process -
The child process is a duplicate of the parent process
Or
The child process has a new program loaded into it.

<!-- slide 25 -->
### Process creation in UNIX
System Call: offers the services of the operating system to the user programs.
fork(): create a new process, which becomes the child process of the caller
exec(): runs an executable file , replacing the previous executable
wait(): suspends execution of the current process until one of its children terminates.
[image]
Fig: Process creation using fork() system call

<!-- slide 26 -->
[image]

<!-- slide 27 -->
int main(){
fork();
fork();
printf(“A”);
}
int main(){
fork();
fork();
fork();
printf(“A”);
}

<!-- slide 28 -->
int main(){
a = fork();
if(a==0) fork();
fork();
printf(“A”);
}
int main(){
fork();
a = fork();
if(a==0) fork();
printf(“A”);
}

<!-- slide 29 -->
main:
x = 1
a = fork()
if (a == 0) {
x = x - 1
print("value of x is:", x)
}
else if (a > 0) {
wait(NULL)
x = x + 1
print("value of x is:", x)
}

<!-- slide 30 -->
main:
x = 1
a = fork()
if (a == 0) {
print(“Child”)
x = x - 1
}
else if (a > 0) {
print(“Parent”)
x = x + 1
wait(NULL)
x = x + 1
}
print("value of x is:", x)

<!-- slide 31 -->
### Process Termination
Process executes last statement and then asks the operating system to delete it using the exit() system call
  Returns status data from child to parent (via wait())
  Process’ resources are deallocated by operating system
Parent may terminate the execution of children processes using the abort() system call. Some reasons for doing so:
  Child has exceeded allocated resources
  Task assigned to child is no longer required
  The parent is exiting, and the operating systems does not allow a child to continue if its parent terminates
Some operating systems do not allow child to exists if its parent has terminated. If a process terminates, then all its children must also be terminated.
  Cascading termination: All children, grandchildren, etc., are terminated.
  The termination is initiated by the operating system.
The parent process may wait for termination of a child process by using the wait() system call. The call returns status information and the id of the terminated process
pid = wait(&status);
If no parent waiting (did not invoke wait()) process is a zombie
If parent terminated without invoking wait(), process is an orphan

<!-- slide 32 -->
### Operating Systems Interprocess Communication
FBA

<!-- slide 33 -->
### Processes in the system
Processes running concurrently may be -
Independent (cannot affect or be affected by other process)
Or
Cooperating (can affect or be affected by other process)
Process cooperation is needed for -
Information sharing
Computational speedup
Modularity
Convenience

<!-- slide 34 -->
### Inter Process Communication
IPC is a mechanism to exchange data and information among processes.
Two fundamental model of IPC -
Shared Memory
Message Passing
[image]

<!-- slide 35 -->
### Shared Memory System (Producer-Consumer Problem)
Producer: produces products for consumer
Consumer: consumes products provided by producer
Share Space /
Buffer
Produce
Consume
Producer
Consumer

<!-- slide 36 -->
### Producer-Consumer Problem (Producer)
[image]
in: next free position in buffer
out: first full position in buffer
Both initialized with 0.
in = 0
out = 0
Here, BUFFER_SIZE = 7
When buffer is full,
in = 6 , out = 0
[0] [1] [2] [3] [4] [5] [6]
When buffer is not full,
In = 4, out = 0
[0] [1] [2] [3] [4] [5] [6]

<!-- slide 37 -->
### Producer-Consumer Problem (Consumer)
[image]
in: next free position in buffer
out: first full position in buffer
Both initialized with 0.
in = 0
out = 0
Here, BUFFER_SIZE = 7
When buffer is empty,
in = 0 , out = 0
[0] [1] [2] [3] [4] [5] [6]
When buffer is not empty,
In = 5, out = 0
[0] [1] [2] [3] [4] [5] [6]

<!-- slide 38 -->
### Message Passing System
If processes P and Q want to communicate, they must send messages to and receive messages from each other.
A communication link must exist between P and Q.
M1
M2
M3
M4
P
Q
send(message)
receive(message)
Useful for exchanging small amount of data
More suited for distributed systems than shared memory
