# CSE321 Module 01: Process Management — sidecar (read this, not the .pptx)

Lecture slides covering process concepts, process states, Process Control Block (PCB), context switching, process scheduling queues, process creation/termination (`fork`, `exec`, `wait`, `exit`), and Inter-Process Communication (IPC).
Pptx last modified: 11-10-2025.

## Content

### Slides 1–4: Process Concept
- **Definition:** A process is a program in execution. A program is a passive entity stored on disk (executable file); a process is an active entity with a program counter and associated resources.
- **Process in Memory consists of:**
  - **Text section:** Executable code.
  - **Data section:** Global variables.
  - **Heap section:** Dynamically allocated memory during runtime (`malloc`, `new`).
  - **Stack section:** Temporary data (function parameters, return addresses, local variables).
  - Current activity represented by **Program Counter (PC)** and processor registers.

### Slides 5–6: Process States & Diagram
A process transitions through the following 5 states:
1. **New:** The process is being created.
2. **Ready:** The process is waiting to be assigned to a processor.
3. **Running:** Instructions are being executed by the CPU.
4. **Waiting (Blocked):** The process is waiting for some event to occur (e.g., I/O completion or reception of a signal).
5. **Terminated:** The process has finished execution.
- *Transitions:* New $ightarrow$ Ready (admitted); Ready $ightarrow$ Running (scheduler dispatch); Running $ightarrow$ Ready (interrupt / timeout); Running $ightarrow$ Waiting (I/O or event wait); Waiting $ightarrow$ Ready (I/O or event completion); Running $ightarrow$ Terminated (exit).

### Slides 7–9: Process Control Block (PCB)
Each process is represented in the operating system by a Process Control Block (also called Task Control Block):
- **Process state:** Ready, running, waiting, etc.
- **Program counter:** Address of next instruction to execute.
- **CPU registers:** Accumulators, index registers, stack pointers, general-purpose registers.
- **CPU scheduling information:** Priority, scheduling queue pointers, parameters.
- **Memory-management information:** Base/limit registers, page tables, segment tables.
- **Accounting information:** Amount of CPU and real time used, time limits, process numbers.
- **I/O status information:** List of I/O devices allocated, list of open files.

### Slides 10–13: CPU Switch / Context Switch
- When CPU switches to another process, the system must **save the state** of the old process and **load the saved state** for the new process via a **context switch**.
- Context of a process is represented in its PCB.
- Context-switch time is pure overhead; the system does no useful work while switching.
- Speed depends on hardware support (e.g., multiple register sets).

### Slides 14–17: Process Scheduling Queues & Schedulers
- **Scheduling Queues:**
  - **Job queue:** Set of all processes in the system.
  - **Ready queue:** Set of all processes residing in main memory, ready and waiting to execute.
  - **Device queues:** Set of processes waiting for a particular I/O device.
- **Schedulers:**
  - **Long-term scheduler (Job scheduler):** Selects processes from pool/spool on disk and loads them into memory for execution. Controls the **degree of multiprogramming**. Invoked infrequently (seconds, minutes). Strives for a good **process mix** of I/O-bound and CPU-bound processes.
  - **Short-term scheduler (CPU scheduler):** Selects from among the processes ready to execute and allocates CPU to one of them. Invoked very frequently (milliseconds) $ightarrow$ must be fast.
  - **Medium-term scheduler:** Removes processes from memory (and active contention for CPU) via **swapping** to disk to reduce degree of multiprogramming, and re-introduces them later.

### Slides 18–20: Process Types
- **I/O-bound process:** Spends more time doing I/O than computations; has many short CPU bursts.
- **CPU-bound process:** Generates I/O requests infrequently; spends more time doing computations; has long CPU bursts.

### Slides 21–28: Process Creation & Operations
- Parent process creates children processes, forming a **process tree** identified via **Process Identifier (PID)**.
- **Resource sharing options:**
  - Parent and children share all resources.
  - Children share subset of parent's resources.
  - Parent and child share no resources.
- **Execution options:**
  - Parent and children execute concurrently.
  - Parent waits until children terminate (`wait()`).
- **Address space options:**
  - Child is a duplicate of parent (same program and data).
  - Child has a new program loaded into it (`exec()`).
- **UNIX System Calls:**
  - `fork()`: Creates a new child process. Returns `0` to child process, child PID ($>0$) to parent process, and `-1` on failure.
  - `exec()`: Family of system calls (`execlp`, `execvp`, etc.) replacing the process's memory space with a new program.
  - `wait()`: Parent suspends execution until a child terminates.
  - `exit()`: Process terminates execution, returning status data to parent.

### Slides 29–31: Process Termination & Special States
- Process terminates when it finishes executing its final statement and asks OS to delete it using `exit()`.
- **Cascading Termination:** If a parent terminates, some operating systems automatically terminate all its children.
- **Zombie Process:** A process that has terminated, but whose parent has not yet called `wait()`. Its entry in the process table remains.
- **Orphan Process:** A process whose parent process terminated without calling `wait()`. In UNIX, orphans are adopted by `init` (or `systemd`, PID 1), which periodically invokes `wait()`.

### Slides 32–38: Inter-Process Communication (IPC)
- Processes executing concurrently may be **independent** (cannot affect or be affected by other processes) or **cooperating** (can affect or be affected by other processes).
- **Reasons for cooperating processes:** Information sharing, computation speedup, modularity, convenience.
- **Two Fundamental Models of IPC:**
  1. **Shared Memory:**
     - A region of memory shared by cooperating processes is established.
     - Processes exchange information by reading and writing data to shared region.
     - Fastest mechanism (memory speed, no OS intervention after setup).
     - Cache coherency and synchronization issues must be handled by application programmer.
  2. **Message Passing:**
     - Communication takes place by means of messages exchanged between cooperating processes.
     - Useful for exchanging smaller amounts of data and easier to implement in distributed environments.
     - System calls: `send(message)` and `receive(message)`.
     - Communication links: Direct (explicit naming: `send(P, message)`) vs Indirect (mailboxes/ports: `send(A, message)`).
     - Synchronization: Blocking (synchronous) vs Non-blocking (asynchronous) send/receive.
     - Buffering: Zero capacity (rendezvous), Bounded capacity, Unbounded capacity.
