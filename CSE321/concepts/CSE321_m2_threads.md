---
type: module-notes
course: CSE321
module: 2
topics: "6–10"
title: Threads
status: complete
date: 2026-06-18
exam: midterm
---

# CSE321 — Module 2: Threads
**Topics 6–10 | Exam: Midterm | Silberschatz Ch.4 | OSTEP Ch.26–27**

---

## Topic 6 — Thread Overview: User vs Kernel Threads, Benefits
**Ref: Silberschatz 4.1, 4.2**

### What is a Thread?
A **thread** is the basic unit of CPU utilization within a process. Every thread has:

| Thread-private | Shared with all threads in the process |
|---|---|
| Thread ID | Code segment (text) |
| Program counter | Data segment (global variables) |
| Register set | Heap (dynamically allocated memory) |
| Stack (local variables) | Open files |
| | Signals |

A traditional (single-threaded) process = one thread. A **multithreaded** process has multiple threads all executing within the same process address space.

```
Single-threaded process:        Multithreaded process:
┌─────────────────┐             ┌──────────────────────────────────┐
│ Code            │             │ Code                             │
│ Data            │             │ Data          (shared)           │
│ Files           │             │ Files                            │
│─────────────────│             │──────────────────────────────────│
│ Stack           │             │ Stack │ Stack │ Stack │ Stack    │
│ Registers       │             │ Regs  │ Regs  │ Regs  │ Regs    │
│ PC              │             │ PC    │ PC    │ PC    │ PC      │
└─────────────────┘             └──────────────────────────────────┘
   (one thread)                    Thread1 Thread2 Thread3 Thread4
```

### Benefits of Multithreading

| Benefit | Explanation |
|---|---|
| **Responsiveness** | One thread can keep UI responsive while another does computation (e.g., browser loading page while still accepting clicks) |
| **Resource sharing** | Threads share memory by default — no IPC overhead for communication within a process |
| **Economy** | Creating a thread is much cheaper than fork() (no new address space allocation). Context switching between threads is faster than between processes |
| **Scalability** | Threads can run truly in parallel on multi-core CPUs; a single-threaded process uses only one core |

### User Threads vs Kernel Threads

| | User Threads | Kernel Threads |
|---|---|---|
| Managed by | User-level library (e.g., pthreads in user space) | OS kernel |
| Creation speed | Fast (no system call) | Slow (requires system call) |
| Context switch | Fast (library handles it) | Slow (kernel involvement) |
| Blocking | One blocking call blocks ALL user threads in the process | Only the blocking thread is blocked |
| Parallelism | Cannot run truly in parallel (all map to one kernel entity) | Can run in parallel on multiple cores |
| Examples | Green threads (early Java), GNU Portable Threads | Linux POSIX threads (NPTL), Windows threads |

> **Key insight:** User threads are fast but fragile — a single blocking system call stalls every thread. Kernel threads are slower to create but the OS can independently schedule each one.

### Multicore Programming (slide-confirmed, missing from original note)

**Concurrency vs. Parallelism — classic exam distinction:**

| | Concurrency | Parallelism |
|---|---|---|
| Definition | Multiple tasks make progress, possibly by interleaving on a single core | Multiple tasks run literally at the same instant, on multiple cores |
| Requires multi-core? | No | Yes |

**Data parallelism:** the same operation is distributed across subsets of data, each on a different core (e.g., summing different chunks of one array in parallel).

**Task parallelism:** different, independent tasks (not the same operation) are distributed across cores.

**5 challenges of multicore programming** (slide list — worth reproducing exactly if asked to enumerate):
1. Dividing activities — finding tasks that can run concurrently
2. Balance — giving each task roughly equal-sized work
3. Data splitting — dividing data to match how tasks are divided
4. Data dependency — ensuring one task's execution doesn't need to wait on another's data
5. Testing and debugging — inherently harder than single-threaded code (non-deterministic interleaving)

---

## Topic 7 — Multithreading Models
**Ref: Silberschatz 4.3**

The relationship between user threads (UTs) and kernel threads (KTs) determines the threading model.

### Many-to-One
```
UT1 ─┐
UT2 ─┤──► KT1
UT3 ─┘
```
- Many user threads mapped to a single kernel thread
- Thread management done in user space: fast
- **Problem 1:** If any thread makes a blocking system call, the entire process blocks
- **Problem 2:** Cannot utilize multiple processors (only one KT)
- Example: Solaris Green Threads (historical)

### One-to-One
```
UT1 ──► KT1
UT2 ──► KT2
UT3 ──► KT3
```
- Each user thread maps to one kernel thread
- True parallelism possible on multi-core
- Blocking in one thread does not affect others
- **Problem:** Creating a user thread requires creating a kernel thread — overhead; OS may limit number of KTs
- Example: **Linux (NPTL), Windows** — this is the dominant model today

### Many-to-Many
```
UT1 ─┐       ┌─► KT1
UT2 ─┤  M×N  │
UT3 ─┼──────►├─► KT2
UT4 ─┤       │
UT5 ─┘       └─► KT3
```
- Many user threads multiplexed onto ≤ that many kernel threads
- True parallelism without the per-thread kernel overhead
- Scheduler activations: kernel informs user library of blocking events so library can reschedule
- **Problem:** Complex implementation
- Example: Solaris (older versions), HP-UX

### Two-Level Model
- Variant of many-to-many: allows a specific UT to be bound to a dedicated KT
- Combines flexibility of M:M with pinning capability of 1:1

| Model | Parallelism | One block blocks all? | Overhead |
|---|---|---|---|
| Many-to-One | No | Yes | Low |
| One-to-One | Yes | No | High (one KT per UT) |
| Many-to-Many | Yes | No | Moderate |

---

## Topic 8 — Thread Libraries: pthreads
**Ref: Silberschatz 4.4.1**

### POSIX Threads (pthreads)
POSIX standard (IEEE 1003.1c) defines the API. Implementation may be in user or kernel space.

### Core Functions

```c
#include <pthread.h>

// Create a thread
int pthread_create(
    pthread_t *thread,        // output: thread ID
    const pthread_attr_t *attr, // thread attributes (NULL = defaults)
    void *(*start_routine)(void *), // function to run
    void *arg                 // argument to pass to function
);

// Wait for thread to finish
int pthread_join(
    pthread_t thread,         // thread to wait for
    void **retval             // output: thread's return value
);

// Terminate calling thread
void pthread_exit(void *retval);
```

### Minimal Example

```c
#include <pthread.h>
#include <stdio.h>

void *runner(void *param) {
    int n = *(int *)param;
    printf("Thread running with n = %d\n", n);
    pthread_exit(0);
}

int main() {
    pthread_t tid;
    pthread_attr_t attr;
    int value = 5;

    pthread_attr_init(&attr);                        // default attributes
    pthread_create(&tid, &attr, runner, &value);     // create thread
    pthread_join(tid, NULL);                         // wait for it
    return 0;
}
```

**Execution flow:**
1. `main()` starts (main thread)
2. `pthread_create()` launches `runner()` in a new thread
3. Both threads run concurrently (on same or different cores)
4. `pthread_join()` causes main thread to block until `runner()` calls `pthread_exit()` or returns

> **Exam note:** Know what each of the 3 core functions does and its parameter types. A question may give you a pthreads code snippet and ask what it outputs or how many threads exist at a given point.

> **Slide-verification note:** the slides also use **generic pseudocode** thread calls (`thread_create`, `thread_join`, `thread_exit`) in several predict-the-output practice examples, not just real `pthread_*` syntax. Recognize both styles — the underlying create/join/exit semantics are identical either way.

---

## Topic 9 — Implicit Threading and Thread Pools
**Ref: Silberschatz 4.5.1**

### Implicit Threading
The difficulty of manually managing threads grows with program complexity. **Implicit threading** moves thread creation/management responsibility to compilers and runtime libraries. The programmer identifies *parallel tasks*, not threads.

**Slide's 5 named methods** (the note previously listed only 3 — Fork-Join was missing):
1. **Thread Pools** (see below)
2. **Fork-Join** — a parent task splits ("forks") into subtasks that run in parallel, then waits and merges ("joins") their results before continuing. Common in divide-and-conquer parallel algorithms.
3. OpenMP (`#pragma omp parallel`)
4. Grand Central Dispatch (Apple)
5. Intel TBB (Threading Building Blocks)

### Thread Pools

**Motivation:** Creating a thread for every request is expensive (time + memory). If a server spawns a thread per HTTP request, a request flood can exhaust system resources.

**Thread Pool pattern:**
1. At startup, create a fixed number of threads and put them in the pool (they sleep waiting for work)
2. When a request arrives, assign it to an idle thread from the pool
3. Thread executes the task, then returns to the pool (does not terminate)
4. If all threads are busy, new requests wait in a queue

```
Incoming requests ──► [Work Queue] ──► [Thread Pool]
                                         T1 T2 T3 T4
                                         (idle or working)
```

**Advantages:**
- Faster response: no thread creation overhead per request
- Bounded resource usage: thread count is capped
- Separation of task submission from execution

**Disadvantages:**
- Pool size is a tuning parameter: too small = requests pile up; too large = memory/scheduling overhead

---

## Topic 10 — Threading Issues: fork/exec, Signal Handling, Cancellation
**Ref: Silberschatz 4.6.1–4.6.3**

### fork() Semantics in Multithreaded Programs

When a multithreaded process calls `fork()`, two UNIX behaviors exist:

| Version | What child gets |
|---|---|
| **Duplicate all threads** | Child is an exact copy — all threads replicated |
| **Duplicate calling thread only** | Child has only the one thread that called fork() |

Linux/POSIX default: **duplicate calling thread only** in the child.

Which to use:
- If child immediately calls `exec()` → duplicate calling thread only (exec replaces everything anyway)
- If child continues running the same program → duplicate all threads

### Signal Handling

A **signal** is a notification sent to a process to indicate an event (e.g., SIGINT from Ctrl+C, SIGSEGV from segfault, SIGALRM from timer).

In a single-threaded program: signal delivered to the process, handled by its one thread.

In a multithreaded program, where does the signal go?

| Option | Description |
|---|---|
| Thread that caused the signal | Appropriate for synchronous signals (SIGSEGV — the faulting thread gets it) |
| Every thread | Every thread receives the signal |
| Certain threads | Some threads block the signal (via signal mask); others handle it |
| Dedicated signal handler thread | One designated thread handles all signals |

UNIX rule: **synchronous signals** (caused by thread's own action) go to the thread that caused them. **Asynchronous signals** (external events like Ctrl+C) can go to any thread that hasn't blocked that signal.

> **Slide-verification note:** the slide labels these same two options as **"(Synchronous cancellation)"** for "deliver to the thread that caused it" and **"(Asynchronous cancellation)"** for "deliver to every thread" — this is the instructor's exact wording (likely a slide typo reusing "cancellation" terminology from the next sub-topic instead of "signal"). If a question quotes the slide directly, recognize this phrasing maps to the synchronous/asynchronous signal distinction above, not to the separate thread-cancellation modes covered next.

### Thread Cancellation

Cancelling a thread before it has finished its task. Two modes:

| Mode | Behavior | Risk |
|---|---|---|
| **Asynchronous cancellation** | Target thread is terminated immediately at any point | Unsafe — thread may hold a mutex, have partially updated shared data, or have unfreed resources |
| **Deferred cancellation** | Thread periodically checks a cancellation flag at defined **cancellation points** (e.g., calls to pthread_testcancel()) | Safe — thread cleans up before terminating |

**Default in pthreads:** deferred cancellation.

```c
// Disable cancellation
pthread_setcancelstate(PTHREAD_CANCEL_DISABLE, NULL);

// Re-enable and set deferred mode
pthread_setcancelstate(PTHREAD_CANCEL_ENABLE, NULL);
pthread_setcanceltype(PTHREAD_CANCEL_DEFERRED, NULL);

// Explicit cancellation point
pthread_testcancel();
```

> **Exam note:** Asynchronous cancellation is unsafe because it can leave shared resources in an inconsistent state. Deferred cancellation is the safe default.

### Thread-Specific Data (slide-confirmed, missing from original note)

Most data is shared across all threads in a process, but each thread sometimes needs its own private copy of a variable independent of the others (e.g., a unique transaction ID per thread in a thread pool, where the thread may be reused for different transactions over time). Most threading libraries provide a **thread-specific data** API for this (e.g., pthreads' `pthread_key_create`/`pthread_setspecific`/`pthread_getspecific`).

> **Exam note:** This is the 5th item in the slide's threading-issues list (alongside fork/exec semantics, signal handling, cancellation, and thread pools) — know it exists and why it's needed even without memorizing the exact API.

---

## Module 2 — Quick Review

| # | Topic | Must Know |
|---|---|---|
| 6 | Thread concept | Thread-private vs shared; 4 benefits; user vs kernel thread tradeoffs |
| 7 | Threading models | Many-to-one (no parallelism), One-to-one (Linux default), Many-to-many (flexible) |
| 8 | pthreads | pthread_create, pthread_join, pthread_exit — parameters and purpose |
| 9 | Thread pools | Why pools exist; idle-thread-reuse pattern; bounded resource usage |
| 10 | Threading issues | fork() ambiguity; signal routing; async (unsafe) vs deferred (safe) cancellation |

**Connection forward:** Threads sharing memory → race conditions → synchronization problem (Module 4).
