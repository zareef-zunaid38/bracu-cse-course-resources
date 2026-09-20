---
source: CSE321_theory_module02_threads.pptx (the original, included in this repo)
fidelity: the summary above the divider was rewritten from the deck on 2026-09-20 after an audit found errors in the earlier version; it is still a condensed paraphrase and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 14 embedded image(s); informational images are described in the summary)
source-mtime: 2025-10-21T00:57:08
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Module 02: Threads

Lecture slides (23) on threads: what a thread is and its benefits, multicore programming, user and kernel threads, the four multithreading models, thread libraries (Pthreads, Java, pseudocode examples), implicit threading (thread pools) and threading issues (`fork()`/`exec()`, cancellation, signal handling). Sections in the deck: Threads (1–5), Multithreading Models (6–9), Thread Libraries (10–16), then Implicit Threading and threading issues (17–23).
Pptx last modified: 21-10-2025.
Rewritten 2026-09-20 directly from the deck (an audit found the earlier summary unreliable). **Amdahl's Law is not in this deck**; the earlier "Amdahl's Law" section was not from these slides. Diagram and code content was read from renders of slides 2, 4, 5, 7–9, 13–16 and 18.

## Content

### Slides 2–5: Threads and multicore programming
- **Slide 2:** a thread is a path of execution within a process. A thread has its own thread ID, program counter, register set and stack, and shares the code section, data section and OS resources with other threads of the same process. A traditional process has a single thread of control (single-threaded); a process with several can do more than one task at a time (multithreaded). Diagram: a single-threaded process (code/data/files shared; one set of registers, PC, stack) beside a multithreaded one (three sets of registers/PC/stack, three thread lines).
- **Slide 3, four benefits of multithreading:** *responsiveness* (one thread can respond quickly while others are blocked or busy computing); *resource sharing* (threads share code, data and resources by default, so several tasks run in one address space); *economy* (creating and managing threads, and switching between them, is much faster than for processes); *scalability* (a single-threaded process runs on one CPU however many exist, a multithreaded one can be spread over processors; single-threaded processes still benefit when several processes contend for the CPU).
- **Slide 4, multicore programming:** multicore/multiprocessor systems pressure programmers; challenges: dividing activities, balance, data splitting, data dependency, testing and debugging. *Parallelism* = performing more than one task simultaneously; *concurrency* = more than one task making progress (on a single core the scheduler provides concurrency). Diagrams: single core interleaves T1, T2, T3, T4, T1, T2, T3, T4, T1…; on two cores, core 1 runs T1, T3, T1, T3, T1… while core 2 runs T2, T4, T2, T4, T2… at the same time.
- **Slide 5:** *data parallelism* — subsets of the same data go to different cores, the same operation on each; *task parallelism* — threads go to different cores, each doing a unique operation. Diagram: one block of data split across cores 0–3 (data parallel) vs the whole data feeding four cores each running a different task (task parallel).

### Slides 6–9: Multithreading models
- **Slide 7, user and kernel threads:** user threads are supported above the kernel without kernel support (what application programmers put in programs; examples POSIX Pthreads, Windows threads, Java threads); kernel threads are supported within the OS kernel (all modern OSs — Windows, Linux, Mac OS X, iOS, Android — allowing simultaneous tasks and simultaneous system-call servicing). In an implementation, user threads must be mapped to kernel threads. Diagram: user threads in user space over kernel threads in kernel space.
- **Slide 8:** **Many-to-One** — many user threads to one kernel thread; one thread blocking blocks all; threads cannot run in parallel on multicore because only one may be in the kernel at a time; examples Solaris Green Threads, GNU Portable Threads. **One-to-One** — each user thread maps to a kernel thread; more concurrency than many-to-one; the number of threads per process is sometimes restricted because of overhead; examples Windows, Linux.
- **Slide 9:** **Many-to-Many** — many user threads mapped to many kernel threads, letting the OS create enough kernel threads; Windows with the ThreadFiber package, otherwise not very common. **Two-level model** — like many-to-many but a user thread can also be bound to a kernel thread. (Each model has a mapping diagram: user threads above kernel threads with lines between them.)

### Slides 10–16: Thread libraries and examples
- **Slide 11:** thread libraries give programmers an API to create and manage threads; implemented in user space (API functions wholly in user space, no kernel support) or in kernel space (system calls, needs kernel support). Three main libraries: POSIX Pthreads (user- or kernel-level, an extension to POSIX), Win32 threads (kernel-level on Windows), Java threads (based on whatever the JVM's OS and hardware provide — Pthreads or Win32).
- **Slide 12, Pthreads:** POSIX IEEE 1003.1c defines the specification, not the implementation; available on Solaris, Linux, Mac OS X, Tru64 and (shareware) Windows; global variables are shared among all threads; one thread can wait for the others to rejoin; execution starts in a specified function; common on UNIX (Linux, Mac OS X).
- **Slide 13, Java threads:** managed by the JVM, typically implemented with the underlying OS's thread model; created by extending the `Thread` class or implementing the `Runnable` interface (`public interface Runnable { public abstract void run(); }`); standard practice is to implement `Runnable`.
- **Slide 14, create a thread** (pseudocode): `THREAD t1; main: thread_create(t1, funcThread, NULL); thread_join(t1, NULL); return; funcThread(arg): print("Entered thread:"); for (i = 0 to 2) { print("thread: ", i) } print("Done with thread ....")`. Output: `Entered thread:` / `thread: 0` / `thread: 1` / `thread: 2` / `Done with thread ....`
- **Slide 15, return values from a thread function:** `THREAD t1; int t_ret; main: n = 5; thread_create(t1, func_thread, n); thread_join(t1, t_ret); print("Thread returned: ", t_ret); return; func_thread(v): v = v * 5; return v`. Output: `Thread returned: 25`.
- **Slide 16, return values by cancelling the thread:** `main: num = 5; thread_create(t1, func_thread, num); thread_join(t1, t_ret); print("Thread returned: ", t_ret)`; `func_thread(n): print("Entered in Thread:"); if (n % 2 == 0) { thread_exit(n * n); print("Operation completed") } else { thread_exit(n * n * n); print("Operation completed") }`. Output: `Entered in Thread:` / `Thread returned: 125` (the "Operation completed" lines never print because `thread_exit` ends the thread first).

### Slides 17–23: Implicit threading and threading issues
- **Slide 17, implicit threading:** shifts the burden of the multicore challenges (dividing activities, balance, data splitting, data dependency, testing/debugging) from the application programmer to compilers and run-time libraries; growing in popularity because correctness gets harder as thread counts rise; five methods explored — thread pools, fork-join, OpenMP, Grand Central Dispatch, Intel Threading Building Blocks.
- **Slide 18, thread pool:** create a number of threads in a pool where they await work. Advantages: servicing a request with an existing thread is usually slightly faster than creating a new one; the number of threads in the application(s) is bounded by the pool size; separating the task from the mechanics of creating it allows different run strategies (e.g. periodic scheduling). The Windows API supports pools: `DWORD WINAPI PoolFunction(AVOID Param) { /* this function runs as a separate thread. */ }`.
- **Slide 19, `fork()` and `exec()`:** `fork()` creates a separate duplicate process; `exec()` replaces the entire process, including all threads, with the named program. Issue: if one thread calls `fork()`, does the new process duplicate all threads or only the caller? Some UNIX systems have two versions of `fork()`. If `exec()` follows immediately, duplicating all threads is unnecessary; if the new process does not call `exec()`, it should duplicate all threads.
- **Slide 20, thread cancellation:** terminating a thread (the *target thread*) before it finishes. *Asynchronous* — one thread terminates the target immediately (a problem if it is in the middle of updating shared data). *Deferred* — the target periodically checks a flag and is cancelled only at a safe point.
- **Slides 21–22, signal handling:** UNIX signals notify a process that an event occurred (synchronous or asynchronous); a signal is generated by an event, delivered to a process and then handled by a default handler or a user-defined handler. In a multithreaded program, delivery options: to the thread the signal applies to (the slide labels this "synchronous cancellation"), to every thread in the process (labelled "asynchronous cancellation"), to certain threads, or to one thread assigned to receive all signals for the process.
- **Slide 23, threading issues (summary):** `fork()`/`exec()` — duplicate all threads or not; thread cancellation; signal handling — where to deliver; thread pool — create a number of threads at process start-up; thread-specific data — each thread may need its own copy of certain data.

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets; list numbering, formatting and images are not reproduced -->

<!-- slide 1 -->
### OPERATING SYSTEMS Threads
FBA

<!-- slide 2 -->
### Thread
A thread is a path of execution within a process.
A thread contains -
  Thread ID
  Program Counter
  Register Set
  Stack
Shares with other threads belonging to the same process -
  Code Section
  Data Section
  OS resources
A traditional process has a single thread of control (Single Threaded Process)
Process with multiple threads of control, can perform more than one task at a time (Multi Threaded Process)
[image]

<!-- slide 3 -->
### Benefits
There are four major categories of benefits to multi-threading:
Responsiveness - One thread may provide rapid response while other threads are blocked or slowed down doing intensive calculations.
Resource sharing - By default threads share common code, data, and other resources, which allows multiple tasks to be performed simultaneously in a single address space.
Economy - Creating and managing threads ( and context switches between them ) is much faster than performing the same tasks for processes.
Scalability, i.e. Utilization of multiprocessor architectures - A single threaded process can only run on one CPU, no matter how many may be available, whereas the execution of a multi-threaded application may be split amongst available processors. ( Note that single threaded processes can still benefit from multi-processor architectures when there are multiple processes contending for the CPU, i.e. when the load average is above some certain threshold. )

<!-- slide 4 -->
### Multicore Programming
Multicore or multiprocessor systems putting pressure on programmers, challenges include:
Dividing activities
Balance
Data splitting
Data dependency
Testing and debugging
Parallelism implies a system can perform more than one task simultaneously
Concurrency supports more than one task making progress
Single processor / core, scheduler providing concurrency
Concurrent execution on single-core system:
[image]
Parallelism on a multi-core system:
[image]

<!-- slide 5 -->
### Multicore Programming
Data parallelism – distributes subsets of the same data across multiple cores, same operation on each
Task parallelism – distributes threads across cores, each thread performing unique operation
[image]

<!-- slide 6 -->
### OPERATING SYSTEMS Multithreading Models
FBA

<!-- slide 7 -->
### User and Kernel Threads
There are two types of threads to be managed in a modern system: User threads and kernel threads.
In a specific implementation, the user threads must be mapped to kernel threads.
[image]
User threads are supported above the kernel, without kernel support. These are the threads that application programmers would put into their programs.
Examples- POSIX Pthreads, Windows threads, Java threads
Kernel threads are supported within the kernel of the OS itself. All modern OS support kernel level threads, allowing the kernel to perform multiple simultaneous tasks and/or to service multiple kernel system calls simultaneously.
Examples- virtually all general-purpose operating systems, including: Windows, Linux, Mac OS X, iOS, Android

<!-- slide 8 -->
### Multithreading Models
Many-to-One:
Many user-level threads mapped to single kernel thread
One thread blocking causes all to block
Multiple threads may not run in parallel on multicore system because only one may be in kernel at a time
Examples:
  Solaris Green Threads
  GNU Portable Threads
[image]
One to One:
Each user-level thread maps to kernel thread
Creating a user-level thread creates a kernel thread
More concurrency than many-to-one
Number of threads per process sometimes restricted due to overhead
Examples
  Windows
  Linux
[image]

<!-- slide 9 -->
### Multithreading Models
Many-to-Many:
Allows many user level threads to be mapped to many kernel threads
Allows the operating system to create a sufficient number of kernel threads
Windows with the ThreadFiber package
Otherwise not very common
[image]
Two-level Model:
Similar to M:M, except that it allows a user thread to be bound to kernel thread
[image]

<!-- slide 10 -->
### OPERATING SYSTEMS Thread Libraries
FBA

<!-- slide 11 -->
### Thread library
Thread libraries provide programmers with an API for creating and managing threads.
Thread libraries may be implemented either in user space or in kernel space. The former involves API functions implemented solely within user space, with no kernel support. The latter involves system calls, and requires a kernel with thread library support.
There are three main thread libraries in use today:
  POSIX Pthreads - may be provided as either a user or kernel library, as an extension to the POSIX standard.
  Win32 threads - provided as a kernel-level library on Windows systems.
  Java threads - Since Java generally runs on a Java Virtual Machine, the implementation of threads is based upon whatever OS and hardware the JVM is running on, i.e. either Pthreads or Win32 threads depending on the system.

<!-- slide 12 -->
### Pthreads
The POSIX standard ( IEEE 1003.1c ) defines the specification for pThreads, not the implementation.
pThreads are available on Solaris, Linux, Mac OSX, Tru64, and via public domain shareware for Windows.
Global variables are shared amongst all threads.
One thread can wait for the others to rejoin before continuing.
pThreads begin execution in a specified function.
Common in UNIX operating systems (Linux & Mac OS X)

<!-- slide 13 -->
### Java Threads
Java threads are managed by the JVM
Typically implemented using the threads model provided by underlying OS
Java threads may be created by:
  Extending Thread class
  Implementing the Runnable interface
Standard practice is to implement Runnable interface
[image]

<!-- slide 14 -->
THREAD t1
main:
thread_create(t1, funcThread, NULL)
thread_join(t1, NULL)
return
funcThread(arg):
print("Entered thread:")
for (i = 0 to 2){
print("thread: ", i)
}
print("Done with thread ....")
Create Thread :
Output:
[image]

<!-- slide 15 -->
THREAD t1
int t_ret
main:
n = 5
thread_create(t1, func_thread, n)
thread_join(t1, t_ret)
print("Thread returned: ", t_ret)
return
func_thread(v):
v = v * 5
return v
Return values from thread function:
[image]
Output

<!-- slide 16 -->
THREAD t1
int t_ret
main:
num = 5
thread_create(t1, func_thread, num)
thread_join(t1, t_ret)
print("Thread returned: ", t_ret)
return
func_thread(n):
print("Entered in Thread:")
if (n % 2 == 0){
thread_exit(n * n)
print("Operation completed")
}
else{
thread_exit(n * n * n)
print("Operation completed")
}
Return values from thread function by cancelling the thread:
[image]
Output

<!-- slide 17 -->
### Implicit Threading
Motivation:
Shifts the burden of addressing the programming challenges outlined earlier (Dividing activities,Balance,Data splitting,Data dependency,Testing and debugging) , from the application programmer to the compiler and run-time libraries.
Growing in popularity as numbers of threads increase, program correctness more difficult with explicit threads
Creation and management of threads done by compilers and run-time libraries rather than programmers
Five methods explored:
        Thread Pools
        Fork-Join
        OpenMP
        Grand Central Dispatch
        Intel Threading Building Blocks

<!-- slide 18 -->
### Thread Pool
Create a number of threads in a pool where they await work.
Advantages:
  Usually slightly faster to service a request with an existing thread than create a new thread.
  Allows the number of threads in the application(s) to be bound to the size of the pool.
  Separating task to be performed from mechanics of creating task allows different strategies for running task.
    i.e,Tasks could be scheduled to run periodically.
Windows API supports thread pools:
[image]

<!-- slide 19 -->
### The fork() and exec() system calls
fork()
  The fork() system call is used to create a separate, duplicate process
exec()
  When a exec() system call is invoked, the program specified in the parameter to exec() will replace the entire process –including all threads
Issue: If one thread in a program calls fork(), does the new process duplicate all threads or the new process single-threaded?
Solution: Some UNIX systems have chosen to have two versions of fork(), one that duplicates all threads and another that duplicates only the thread that invoked the fork() system call
Which version of fork() will to be used depends on the application
  If exec() is called immediately after forking then duplicating all threads is unnecessary.
  If the separate process does not call exec() after forking, the separate process should duplicate all threads

<!-- slide 20 -->
### Thread Cancellation
Thread cancellation is the task of Terminating a thread before it has completed.
Thread to be canceled is target thread
Two general approaches:
  Asynchronous cancellation: one thread terminates the target thread immediately.
    There is an issue if a thread is cancelled while in the midst of updating data it is sharing with other threads.
  Deferred cancellation allows the target thread to periodically check if it should be cancelled
    Cancellation occurs only after the target thread has checked a flag to determine if it should be cancelled or not. So it should be canceled at a point when it can be cancelled safely.

<!-- slide 21 -->
### Signal Handling
Signals are used in UNIX systems to notify a process that a particular event has occurred.
Synchronous and asynchronous received signal.
A signal handler is used to process signals
    A signal is generated by the occurrence of a particular event
    A generated signal is delivered to a process
    Once delivered, the signal must be handled
Every Signal may be handled by one of two possible handlers:
  A default signal handler
  A user-defined signal handler

<!-- slide 22 -->
### Signal Handling
Signals are always delivered to a process but delivering signals is more complicated in multi-threaded program. In that case following options exist
  Deliver the signal to the thread to which the signal applies (Synchronous cancellation)
  Deliver the signal to every thread in the process (Asynchronous cancellation)
  Deliver the signal to certain threads in the process
  Assign a specific thread to receive all signals for the process

<!-- slide 23 -->
### Threading Issues
fork() and exec() System Calls: Duplicate all the threads or not?
Thread cancellation: Thread cancellation is the task of terminating a thread before it has completed.
Signal Handling: Where should a signal be delivered?
Thread Pool: Create a number of threads at the process start-up.
Thread Specific data: Each thread might need it’s own copy of certain data.
