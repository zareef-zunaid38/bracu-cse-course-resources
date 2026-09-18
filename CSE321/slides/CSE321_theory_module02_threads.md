# CSE321 Module 02: Threads — sidecar (read this, not the .pptx)

Lecture slides covering multithreading concepts, process vs thread comparison, benefits of multithreading, multicore programming (data vs task parallelism, Amdahl's Law), and multithreading models.
Pptx last modified: 21-10-2025.

## Content

### Slides 1–2: Thread Concept
- **Definition:** A thread is a basic unit of CPU utilization; it comprises a thread ID, a program counter (PC), a register set, and a stack.
- It shares with other threads belonging to the same process its **code section**, **data section**, and other operating-system resources (such as open files and signals).
- Traditional heavyweight process has a single thread of control.

### Slides 3–4: Benefits of Multithreading
1. **Responsiveness:** Multithreading an interactive application allows a program to continue running even if part of it is blocked or performing a lengthy operation (e.g., web browser loading image while responding to user clicks).
2. **Resource Sharing:** Threads share the memory and resources of the process by default, avoiding the overhead of explicit IPC or shared memory regions.
3. **Economy:** Allocating memory and resources for process creation is costly. Context-switching between threads is much faster than switching between processes.
4. **Scalability / Utilization of Multiprocessor Architectures:** Threads can run in parallel on different processing cores in a multiprocessor/multicore system.

### Slides 5–9: Multicore Programming & Parallelism vs Concurrency
- **Concurrency vs Parallelism:**
  - *Concurrent system:* Supports more than one task by allowing all tasks to make progress (interleaved execution on single core).
  - *Parallel system:* Performs more than one task simultaneously (simultaneous execution on multiple cores).
- **Types of Parallelism:**
  - **Data Parallelism:** Distributes subsets of the same data across multiple computing cores and performs the same operation on each core (e.g., summing array elements across $N$ cores).
  - **Task Parallelism:** Distributes distinct tasks (threads) across multiple computing cores, where each thread performs a unique operation on the same or different data.

### Slides 10–13: Amdahl's Law
- Formula identifying potential performance gains from adding computing cores to an application that has both serial ($S$) and parallel ($1-S$) components:
  $$\text{Speedup} \le \frac{1}{S + \frac{1 - S}{N}}$$
  where $S$ is the serial portion of the application ($0 \le S \le 1$), and $N$ is the number of processing cores.
- **Key Takeaways:**
  - As $N \to \infty$, speedup converges to $\frac{1}{S}$.
  - If 25% of an application is serial ($S = 0.25$), the maximum speedup on an infinite number of cores is $\frac{1}{0.25} = 4\times$.
  - Serial code is a critical bottleneck in scaling performance on multicore systems.

### Slides 14–16: User Threads vs Kernel Threads
- **User Threads:**
  - Supported above the kernel and managed without kernel support (via user-level thread libraries like POSIX Pthreads, Windows threads, Java threads).
  - Fast to create and manage.
  - Drawback: If kernel is single-threaded, any user thread performing a blocking system call causes the entire process to block.
- **Kernel Threads:**
  - Supported and managed directly by the operating system kernel (e.g., Linux, Windows, macOS, Solaris).
  - Creation and management involve kernel overhead.
  - If one thread blocks, kernel can schedule another thread of the process.

### Slides 17–23: Multithreading Models
Relationships establishing mapping between user threads and kernel threads:
1. **Many-to-One Model:**
   - Maps many user-level threads to one kernel thread.
   - Thread management done by thread library in user space $ightarrow$ efficient.
   - *Drawback:* Entire process blocks if a thread makes a blocking system call. Multiple threads cannot run in parallel on multicore systems (only one thread can access kernel at a time).
   - Examples: Solaris Green Threads, GNU Portable Threads.
2. **One-to-One Model:**
   - Maps each user thread to a kernel thread.
   - Provides more concurrency; when a thread makes a blocking system call, other threads can continue running. Allows multiple threads to run in parallel on multiprocessors.
   - *Drawback:* Creating a user thread requires creating a corresponding kernel thread, which can degrade performance if too many threads are created.
   - Examples: Windows, Linux, Solaris 9 and later.
3. **Many-to-Many Model:**
   - Multiplexes many user-level threads to a smaller or equal number of kernel threads.
   - Developers can create as many user threads as necessary, and corresponding kernel threads can run in parallel on a multiprocessor. When a thread performs a blocking call, kernel can schedule another thread.
4. **Two-Level Model:**
   - A variation of Many-to-Many that still allows a user thread to be bound to a specific kernel thread (e.g., IRIX, HP-UX, Tru64 UNIX, Solaris 8 and earlier).
