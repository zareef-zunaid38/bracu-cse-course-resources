---
type: module-notes
course: CSE321
module: 1
topics: "1–5"
title: Process
status: complete
date: 2026-06-18
exam: midterm
---

# CSE321 — Module 1: Process
**Topics 1–5 | Exam: Midterm | Silberschatz Ch.3**

---

## Topic 1 — Process Concept: PCB, States, State Transitions
**Ref: Silberschatz 3.1.1, 3.1.2 | OSTEP Ch.4**

### What is a Process?
A **process** is a *program in execution*. A program is passive (stored on disk); a process is active (loaded into memory, executing).

One program can create multiple processes (e.g., multiple browser tabs running the same binary).

### Dual Mode Operation
**Ref: slide-confirmed, not in topic map's Silberschatz citation for Topic 1**

The CPU has a **mode bit** distinguishing two execution modes:

| Mode | Bit | What runs |
|---|---|---|
| **User mode** | 1 | User processes — restricted, cannot directly execute privileged instructions |
| **Kernel (monitor) mode** | 0 | OS code — full hardware access |

A **trap** (software interrupt / system call) switches the CPU from user mode to kernel mode so the OS can perform a privileged operation on the user process's behalf, then switches back. This is the enforcement mechanism that keeps user processes from directly touching hardware or other processes' memory.

> **Exam note:** Dual mode is the hardware-level answer to "how does the OS protect itself and other processes from a misbehaving process."

### System Calls
**Ref: slide-confirmed, ~6-slide block between Process Scheduling and Process Operations — not separately numbered in the topic map but present in the deck**

A **system call** is the programming interface through which a process requests a service from the OS kernel — invoking one triggers the trap into kernel mode described above.

**System Call Interface:** user code calls a library function (e.g., `open()`); the library maps it to the correct system call number and traps into the kernel; the kernel performs the operation and returns control to user mode.

**Types of System Calls** (Windows vs. Linux naming, both directions worth recognizing):

| Category | Windows example | Linux example |
|---|---|---|
| **Process Control** | `CreateProcess()`, `ExitProcess()` | `fork()`, `exit()` |
| **File Manipulation** | `CreateFile()`, `ReadFile()` | `open()`, `read()` |
| **Device Manipulation** | `SetConsoleMode()` | `ioctl()` |
| **Information Maintenance** | `GetCurrentProcessID()` | `getpid()` |
| **Communication** | `CreatePipe()` | `pipe()` |
| **Protection** | `SetFileSecurity()` | `chmod()` |

> **Exam note:** A question may give a category (e.g. "Process Control") and ask for the matching system call on either OS, or give a call and ask which category it belongs to.

### Process Control Block (PCB)
The OS represents each process as a PCB — a data structure stored in kernel memory.

| PCB Field | Contents |
|---|---|
| Process state | new / ready / running / waiting / terminated |
| Program counter | address of next instruction to execute |
| CPU registers | all registers (accumulators, index, stack pointer, etc.) — saved on context switch |
| Memory management info | page tables, segment tables, base/limit registers |
| I/O status info | list of open files, I/O devices allocated |
| Accounting info | CPU time used, time limits, job numbers |
| **CPU scheduling info** | **priorities, scheduling queue pointers** — slide-confirmed 6th category, don't drop it when listing PCB fields |

> **Exam note:** Know every field category. A common question is "what is saved during a context switch" — answer: CPU registers + program counter (i.e., the CPU state portion of the PCB).

### Process States

```
  [NEW] ──admitted──► [READY] ──scheduler dispatch──► [RUNNING]
                         ▲                                 │
                         │  I/O or event completion        │ I/O or event wait
                         │                                 ▼
                         └──────────────────────────── [WAITING]
                                                            
  [RUNNING] ──interrupt──► [READY]
  [RUNNING] ──exit──► [TERMINATED]
```

| Transition | Trigger |
|---|---|
| new → ready | Process admitted to memory by OS |
| ready → running | Scheduler dispatches process |
| running → waiting | Process issues I/O request or waits for event |
| waiting → ready | I/O completes or event occurs |
| running → ready | Timer interrupt (preemption) |
| running → terminated | Process calls exit() or is killed |

> **Key distinction:** A process in **waiting** is blocked on I/O — it cannot use the CPU even if the CPU is free. A process in **ready** is capable of running but the CPU is assigned elsewhere.

---

## Topic 2 — Process Scheduling Queues and Context Switching
**Ref: Silberschatz 3.1.3, 3.1.4**

### Scheduling Queues

| Queue | Contains |
|---|---|
| **Job queue** | All processes in the system (including on disk) |
| **Ready queue** | Processes in main memory, ready and waiting for CPU |
| **Device queue** | Processes waiting for a specific I/O device |

A process moves between queues during its lifetime. Each I/O device has its own device queue.

### Schedulers

| Scheduler | Also Called | Frequency | Role |
|---|---|---|---|
| Long-term (job) scheduler | Admission scheduler | Infrequent | Controls degree of multiprogramming (how many processes in memory) |
| Short-term (CPU) scheduler | Dispatcher | Very frequent (milliseconds) | Selects which ready process gets the CPU next |
| Medium-term scheduler | Swapper | Moderate | Swaps processes in/out of memory (virtual memory systems) |

### Context Switch
When the CPU switches from process A to process B:
1. Save A's state into its PCB (registers, PC, memory maps)
2. Load B's state from its PCB
3. Resume B from where it left off

**Context switch time is pure overhead** — no useful work is done during the switch. Typical cost: a few microseconds. Minimizing unnecessary switches is a key OS goal.

> **Exam note:** Context switch overhead is why a very small Round Robin quantum is bad — more time is spent switching than executing.

---

## Topic 3 — Process Operations: fork, Creation, Termination
**Ref: Silberschatz 3.2 | OSTEP Ch.5**

### Process Creation
- `fork()` — system call that creates a child process as an **exact duplicate** of the parent
  - Child gets a copy of parent's address space (code, data, heap, stack, open files)
  - Both parent and child continue executing from the line after `fork()`
  - Return value distinguishes them:
    - **In child:** `fork()` returns `0`
    - **In parent:** `fork()` returns child's PID (a positive integer)
    - **On error:** `fork()` returns `-1`

```c
pid_t pid = fork();
if (pid == 0) {
    // child process
} else if (pid > 0) {
    // parent process; pid = child's PID
} else {
    // fork failed
}
```

**Resource sharing options at creation** (slide-confirmed): parent and child may share **all** resources, share a **subset**, or share **none**. Separately, the child's address space is either a **duplicate of the parent** (fork semantics) or **loaded with a new program** (if followed by exec()) — two independent choices the slide presents explicitly.

- `exec()` — replaces the current process's memory with a new program
  - The calling process does not return from exec() if successful
  - Commonly used after fork() in child to run a different program

- `wait()` — parent blocks until a child terminates and collects its exit status
  - Without wait(): terminated child becomes a **zombie** (PCB retained until parent collects it)

### Process Hierarchy
- UNIX: all processes form a tree rooted at `init` (PID 1)
- Parent PID tracked in PCB as `PPID`

### Process Termination
- `exit(status)` — process requests its own termination; returns status to parent
- Parent can kill a child with `kill(pid, SIGKILL)` (signal 9)
- **`abort()`** (slide-confirmed) — parent terminates a child explicitly. Reasons: child exceeded its allocated resources, the child's assigned task is no longer needed, or the parent itself is exiting.
- **Cascading termination** (slide-confirmed): when a parent terminates, some systems require all its children to terminate too — the OS initiates this top-down.

### Problematic States

| State | Cause | Problem |
|---|---|---|
| **Zombie** | Child exited; parent has not called wait() | PCB remains in memory; entry persists in process table |
| **Orphan** | Parent terminated before child | Child re-parented to init process (init periodically calls wait()) |

> **Exam note:** Know fork() return values. A classic question gives you a program with fork() and asks how many processes are created or what is printed.

---

## Topic 4 — IPC: Shared Memory and Message Passing
**Ref: Silberschatz 3.3**

> **Slide-verification note:** The deck covers basic shared memory and a basic message-passing system, but the **Naming / Synchronization / Buffering variant breakdown below is textbook-only (Silberschatz) — not found on the slides.** Keep it for moderate-difficulty questions (the Questionnaire Prep Strategy doc states these require textbook reading beyond slides), but don't expect it verbatim from lecture.

### Why IPC?
Processes are isolated by default (separate address spaces). **Inter-Process Communication (IPC)** allows cooperating processes to exchange data.

Two fundamental models:

### Model 1 — Shared Memory
- OS creates a shared memory region; both processes map it into their address space
- Processes read/write directly to this region — **no kernel involvement per operation**
- **Fast**: data copy not required between user and kernel space
- **Risk**: processes must explicitly synchronize access (race conditions if not)

```
Process A │ shared memory region │ Process B
          │  [data written here]  │
```

### Model 2 — Message Passing
- Processes communicate via kernel: `send(message)` and `receive(message)`
- Every exchange goes through kernel — **slower** than shared memory (at least 2 system calls per message)
- **Safe**: no explicit synchronization needed; kernel mediates access

| | Shared Memory | Message Passing |
|---|---|---|
| Speed | Fast (no kernel per op) | Slow (kernel per op) |
| Synchronization | Manual (programmer's job) | Automatic |
| Useful when | Large data, same machine | Small data, distributed systems |

### Message Passing Variants

**Naming (how processes find each other):**
- *Direct*: `send(P, msg)` and `receive(Q, msg)` — processes name each other explicitly
- *Indirect*: messages go to/from a **mailbox (port)** — processes name the mailbox, not each other

**Synchronization:**
- *Blocking send* (synchronous): sender blocks until message is received
- *Non-blocking send* (asynchronous): sender sends and continues immediately
- *Blocking receive*: receiver blocks until a message arrives
- *Non-blocking receive*: receiver gets message or null if none

**Buffering:**
- *Zero capacity*: no queue; sender must wait for receiver (rendezvous)
- *Bounded capacity*: queue of fixed size; sender blocks when full
- *Unbounded capacity*: infinite queue; sender never blocks

---

## Topic 5 — IPC Mechanisms: Pipes and Sockets
**Ref: Silberschatz 3.4**

> **Slide-verification note:** This entire topic (pipes, FIFOs, sockets, TCP/UDP) is **not present anywhere in the slide deck** — fully textbook-only. Still in scope per the topic map, but lower priority than slide-confirmed content if revision time is short tonight.

### Pipes
Pipes are a channel through which bytes flow in one direction (by default).

#### Ordinary Pipes
- **Unidirectional** — one write end, one read end
- Only between **parent and child** (related processes)
- Temporary: exist only while processes are running
- In UNIX: `pipe(fd[2])` — `fd[1]` is write end, `fd[0]` is read end

```
Parent ──write──► [pipe buffer] ──read──► Child
```

#### Named Pipes (FIFOs)
- **Bidirectional** capable
- Any two processes can use them (no parent-child relationship required)
- Persistent: exist as a file in the filesystem even after processes exit
- Created with `mkfifo()` system call

| | Ordinary Pipe | Named Pipe (FIFO) |
|---|---|---|
| Directionality | Unidirectional | Bidirectional |
| Process relationship | Parent-child only | Any two processes |
| Lifetime | Process lifetime | Persistent in filesystem |

### Sockets
A **socket** is an endpoint of communication identified by:
```
IP address : Port number
```

Sockets support communication between processes on **different machines** (or same machine).

| Socket Type | Protocol | Characteristics |
|---|---|---|
| **TCP socket** | Connection-oriented | Reliable, ordered delivery, error correction |
| **UDP socket** | Connectionless | No guarantee of delivery or order; lower overhead |

- Well-known ports: HTTP = 80, HTTPS = 443, FTP = 21, SSH = 22
- Ports below 1024 are reserved for system services

> **Exam note:** Pipes are for same-machine, related/unrelated processes. Sockets are for network communication (or local via loopback 127.0.0.1). A socket below port 1024 requires root privileges.

---

## Module 1 — Quick Review

| # | Topic | Must Know |
|---|---|---|
| 1 | PCB & Process States | 5 states, all transitions, PCB fields |
| 2 | Queues & Context Switch | 3 queue types, context switch = overhead |
| 3 | fork / exec / wait | Return values, zombie vs orphan |
| 4 | IPC Models | Shared memory (fast, manual sync) vs message passing (slow, safe) |
| 5 | Pipes & Sockets | Ordinary vs named pipe; TCP vs UDP |

**Prerequisites for next module:** Understand that a process can have multiple threads sharing its code/data/heap but with separate stacks and registers.
