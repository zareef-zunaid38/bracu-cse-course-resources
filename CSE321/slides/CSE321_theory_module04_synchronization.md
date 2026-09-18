# CSE321 Module 04: Process Synchronization — sidecar (read this, not the .pptx)

Lecture slides covering process synchronization, race conditions, critical section problem requirements, Peterson's algorithm, hardware instructions (`TestAndSet`, `CompareAndSwap`), mutex locks, semaphores, and classic synchronization problems.
Pptx last modified: 03-11-2025.

## Content

### Slides 1–6: Background & Race Condition
- Concurrent access to shared data may result in data inconsistency. Maintaining data consistency requires mechanisms to ensure orderly execution of cooperating processes.
- **Producer-Consumer Problem (Bounded Buffer):**
  - Shared integer `counter` tracks number of items in buffer.
  - Producer executes: `counter++` (registers: `register1 = counter`, `register1 = register1 + 1`, `counter = register1`).
  - Consumer executes: `counter--` (registers: `register2 = counter`, `register2 = register2 - 1`, `counter = register2`).
  - If producer and consumer execute concurrently, machine instructions may interleave, leaving `counter` in an incorrect state.
- **Race Condition:** A situation where several processes access and manipulate the same data concurrently, and the outcome of the execution depends on the particular order in which access takes place.

### Slides 7–10: The Critical Section Problem
- Each process has a segment of code called a **Critical Section (CS)** where shared resources are accessed.
- When one process is executing in its critical section, no other process is allowed to execute in its critical section.
- **General Code Structure:**
  ```c
  do {
      // Entry Section
      // Critical Section
      // Exit Section
      // Remainder Section
  } while (true);
  ```
- **Three Requirements for a Solution to the Critical Section Problem:**
  1. **Mutual Exclusion:** If process $P_i$ is executing in its critical section, no other processes can be executing in their critical sections.
  2. **Progress:** If no process is executing in its critical section and some processes wish to enter their critical sections, only those processes not executing in their remainder sections can participate in deciding which will enter next, and this selection cannot be postponed indefinitely.
  3. **Bounded Waiting:** A bound must exist on the number of times other processes are allowed to enter their critical sections after a process has made a request to enter and before that request is granted (prevents starvation).

### Slides 11–16: Peterson's Solution
- Classic software-based solution for two processes ($P_0$ and $P_1$). Assumes `load` and `store` machine instructions are atomic.
- **Shared Variables:**
  - `int turn;` (indicates whose turn it is to enter critical section)
  - `boolean flag[2];` (`flag[i] = true` indicates process $P_i$ is ready to enter critical section; initialized to `false`)
- **Algorithm for Process $P_i$ (other process $P_j$, where $j = 1 - i$):**
  ```c
  do {
      flag[i] = true;
      turn = j;
      while (flag[j] && turn == j);
      
      // Critical Section
      
      flag[i] = false;
      
      // Remainder Section
  } while (true);
  ```
- **Proof of Correctness:**
  - *Mutual Exclusion:* Preserved because $P_0$ and $P_1$ cannot enter CS simultaneously since `turn` can only hold one value ($0$ or $1$) at any instant.
  - *Progress & Bounded Waiting:* Both satisfied; process $P_i$ can be prevented from entering only if stuck in `while` loop with `flag[j] == true` and `turn == j`. Once $P_j$ exits CS, it resets `flag[j] = false`, unblocking $P_i$.

### Slides 17–22: Hardware Support for Synchronization
- Many systems provide hardware instructions to implement synchronization atomically (uninterruptibly):
- **1. TestAndSet (`test_and_set`):**
  ```c
  boolean test_and_set(boolean *target) {
      boolean rv = *target;
      *target = true;
      return rv;
  }
  ```
  - *Mutual exclusion with `test_and_set`:* Shared variable `boolean lock = false;`
    ```c
    do {
        while (test_and_set(&lock));
        // Critical Section
        lock = false;
        // Remainder Section
    } while (true);
    ```
- **2. CompareAndSwap (`compare_and_swap`):**
  ```c
  int compare_and_swap(int *value, int expected, int new_value) {
      int temp = *value;
      if (*value == expected)
          *value = new_value;
      return temp;
  }
  ```
  - *Mutual exclusion with `compare_and_swap`:* Shared variable `int lock = 0;`
    ```c
    do {
        while (compare_and_swap(&lock, 0, 1) != 0);
        // Critical Section
        lock = 0;
        // Remainder Section
    } while (true);
    ```

### Slides 23–26: Mutex Locks
- Hardware-based solutions are complicated; software tools called **Mutex Locks** (mutual exclusion) simplify CS protection.
- Boolean variable `available` indicates if lock is available.
- **Operations:**
  - `acquire()`: Acquires lock (busy waits if unavailable).
  - `release()`: Releases lock.
  ```c
  acquire() {
      while (!available); // busy wait
      available = false;
  }
  release() {
      available = true;
  }
  ```
- **Spinlock:** A mutex lock where waiting process loops continuously in `acquire()`. Advantage: No context-switch overhead for short lock durations; disadvantage: wastes CPU cycles (busy waiting).

### Slides 27–34: Semaphores
- Synchronization tool providing more sophisticated control. An integer variable `S` accessed only via two standard atomic operations: `wait()` (P / proberen) and `signal()` (V / verhogen).
- **Definitions:**
  ```c
  wait(S) {
      while (S <= 0); // busy wait
      S--;
  }
  signal(S) {
      S++;
  }
  ```
- **Types of Semaphores:**
  - **Counting Semaphore:** Integer value can range over an unrestricted domain (used to control access to a given resource with finite instances).
  - **Binary Semaphore:** Integer value ranges only between 0 and 1 (functions identically to a mutex lock).
- **Non-Busy-Waiting Implementation (Block & Wakeup):**
  - Semaphore contains an integer `value` and a `list` of waiting processes (PCBs).
  - `wait(S)`: Decrements `S->value`. If `S->value < 0`, process adds itself to `S->list` and calls `block()`.
  - `signal(S)`: Increments `S->value`. If `S->value <= 0`, removes a process $P$ from `S->list` and calls `wakeup(P)`.

### Slides 35–42: Classic Problems of Synchronization & Deadlock
- **1. Bounded-Buffer (Producer-Consumer) Problem:**
  - Shared: `semaphore mutex = 1; semaphore empty = n; semaphore full = 0;`
  - Producer: `wait(empty); wait(mutex); ... add item ... signal(mutex); signal(full);`
  - Consumer: `wait(full); wait(mutex); ... remove item ... signal(mutex); signal(empty);`
- **2. Readers-Writers Problem:**
  - Multiple readers can read simultaneously, but writer requires exclusive access.
  - Shared: `semaphore rw_mutex = 1; semaphore mutex = 1; int read_count = 0;`
  - Reader: `wait(mutex); read_count++; if (read_count == 1) wait(rw_mutex); signal(mutex); ... reading ... wait(mutex); read_count--; if (read_count == 0) signal(rw_mutex); signal(mutex);`
  - Writer: `wait(rw_mutex); ... writing ... signal(rw_mutex);`
- **3. Dining-Philosophers Problem:**
  - 5 philosophers sitting at a table with 5 chopsticks; need 2 chopsticks to eat.
  - Array of semaphores `semaphore chopstick[5] = {1, 1, 1, 1, 1};`
  - Philosopher $i$: `wait(chopstick[i]); wait(chopstick[(i+1)%5]); ... eat ... signal(chopstick[i]); signal(chopstick[(i+1)%5]);`
  - *Risk:* Deadlock occurs if all philosophers become hungry simultaneously and grab their left chopstick.
