# CSE321 Module 00: Introduction — sidecar (read this, not the .pptx)

Lecture slides covering foundational operating systems concepts, course outcomes, computer system architecture, interrupts, storage hierarchy, and dual-mode operation.
Pptx last modified: 15-06-2025.

## Content

### Slide 1: Course Title & Instructor
- **Course:** CSE321: Operating Systems
- **Module:** Introduction
- **Faculty:** FBA

### Slide 2: Course Outcomes (COs)
1. Explore various aspects of process management in operating systems.
2. Understand how different CPU scheduling algorithms work and their respective importance.
3. Develop practical knowledge on the concepts and behaviors of threads.
4. Inspect process synchronization mechanisms and deadlocks.
5. Analyze the management of main and virtual memory.
6. Understand the concepts and implementations of file systems.
7. Identify security issues and protection mechanisms in operating systems.

### Slide 3: Marks Distribution & Attendance Policy
- **Theory (75%):**
  - Assignment: 5%
  - Quiz: 10% (best $n-1$)
  - Midterm Exam: 25%
  - Final Exam: 35%
- **Lab (25%)**
- **Attendance Rule:**
  - Theory classes: Minimum 70% attendance required to attend final exam.
  - Lab classes: Minimum 90% attendance required to attend final exam.

### Slide 4: What is an Operating System?
- A program that acts as an intermediary between a user of a computer and the computer hardware.
- **Operating system goals:**
  - Execute user programs and make solving user problems easier.
  - Make the computer system convenient to use.
  - Use the computer hardware in an efficient manner.

### Slide 5: Views of Operating System
- **User View:** Focuses on convenience, ease of use, and good performance. In shared systems (e.g., mainframes/servers), maximizes resource utilization.
- **System View:**
  - **Resource Allocator:** Manages all resources (CPU time, memory space, file-storage space, I/O devices) and decides between conflicting requests for fair and efficient resource use.
  - **Control Program:** Controls execution of programs to prevent errors and improper use of the computer.

### Slide 6: Computer System Structure
Divided into 4 components:
1. **Hardware:** Provides basic computing resources (CPU, memory, I/O devices).
2. **Operating System:** Controls and coordinates use of hardware among various applications and users.
3. **Application Programs:** Define the ways in which the system resources are used to solve the computing problems of the users (word processors, compilers, web browsers, database systems, video games).
4. **Users:** People, machines, other computers.

### Slide 7: Computer Startup
- **Bootstrap Program:** Loaded at power-up or reboot.
  - Stored in ROM or EEPROM (generally known as firmware).
  - Initializes all aspects of the system (CPU registers, device controllers, memory contents).
  - Locates the operating system kernel and loads it into memory.
  - Starts execution of the kernel.

### Slide 8: Computer System Operation
- One or more CPUs, device controllers connect through common bus providing access to shared memory.
- Concurrent execution of CPUs and devices competing for memory cycles.
- Memory controller synchronizes access to shared memory.

### Slide 9: Interrupts
- Hardware triggers an interrupt by sending a signal to the CPU (usually via the system bus).
- Software triggers an interrupt by executing a special operation called a **system call** (also called a **monitor call**), **trap**, or **exception** (caused by error like division by zero or invalid memory access).
- **Interrupt Handling:**
  - Operating system preserves the state of the CPU by storing registers and the program counter.
  - Determines which type of interrupt has occurred.
  - Separate segments of code determine what action should be taken for each type of interrupt via the **Interrupt Vector table**.

### Slide 10: I/O Structure
- **Synchronous I/O:** After I/O starts, control returns to user program only upon I/O completion. CPU idles or wait instruction is issued until next interrupt. At most one I/O request is processed at a time.
- **Asynchronous I/O:** After I/O starts, control returns to user program without waiting for I/O completion. System call allows user to wait for I/O completion if needed. Device status table contains entry for each I/O device indicating its type, address, and state.

### Slide 11: Storage Structure & Hierarchy
Storage systems organized in hierarchy based on speed, cost, and volatility:
1. **Registers** (fastest, smallest, volatile, highest cost)
2. **Cache** (SRAM, volatile)
3. **Main Memory** (DRAM, volatile, only large storage media CPU can access directly)
4. **Solid-State Disk** (non-volatile)
5. **Magnetic Disk** (non-volatile, secondary storage)
6. **Optical Disk**
7. **Magnetic Tapes** (slowest, largest capacity, non-volatile, lowest cost)

### Slide 12: Caching
- Information in use copied from slower to faster storage system temporarily.
- Faster storage (cache) checked first to determine if information is there.
  - If found: used directly from cache.
  - If not: data copied to cache and used there.
- Cache smaller than storage being cached.
- Requires cache management policy and coherence protocols (especially in multiprocessor systems).

### Slide 13: Computer System Architecture
- **Single-Processor Systems:** One main CPU capable of executing a general-purpose instruction set.
- **Multiprocessor Systems (Parallel / Multicore Systems):** Two or more processors in close communication sharing computer bus, clock, memory, and peripheral devices.
  - **Advantages:**
    1. Increased throughput (more work done in less time).
    2. Economy of scale (share peripherals, storage, power supplies).
    3. Increased reliability / fault tolerance (graceful degradation or fail-soft).
  - **Types:**
    - *Asymmetric Multiprocessing (AMP):* Boss-worker relationship; boss processor schedules and allocates work to worker processors.
    - *Symmetric Multiprocessing (SMP):* Most common; all processors are peers and perform all tasks within the OS.

### Slide 14: Operating System Structure
- **Multiprogramming (Batch systems):**
  - Needed for efficiency. Single user cannot keep CPU and I/O devices busy at all times.
  - Organizes jobs (code and data) so CPU always has one to execute.
  - Subset of total jobs kept in memory; OS switches to another job when current job has to wait (e.g., for I/O).
- **Timesharing (Multitasking):**
  - Logical extension of multiprogramming.
  - CPU switches jobs so frequently that users can interact with each job while running (interactive computing).
  - Response time should be $< 1$ second.
  - If several jobs ready to run at the same time $ightarrow$ CPU scheduling.
  - If processes don't fit in memory $ightarrow$ swapping and Virtual Memory.

### Slide 15: Operating System Operations — Dual-Mode Operation
- Dual-mode operation provides hardware protection against erroneous or malicious programs.
- **Modes:**
  - **User Mode** (mode bit = 1): Execution on behalf of user application.
  - **Kernel Mode / Supervisor / System / Privileged Mode** (mode bit = 0): Execution on behalf of operating system.
- Mode bit provided by hardware. When a trap or interrupt occurs, hardware switches from user to kernel mode (sets bit to 0). When returning to user program, hardware switches to user mode (sets bit to 1).
- **Privileged Instructions:** Instructions that may cause harm only executable in kernel mode (e.g., direct I/O access, turn off interrupts, modify timer/memory management registers).

### Slide 16: Timer
- Hardware timer prevents user program from getting stuck in an infinite loop or not calling system services and never returning control to OS.
- Timer is set to interrupt the computer after a specified period (fixed or variable rate).
- OS sets counter; counter is decremented by physical clock ticks. When counter reaches zero, a timer interrupt occurs and control transfers automatically to OS.
- Setting the timer is a privileged instruction.
