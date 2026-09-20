---
source: CSE321_theory_module04_synchronization.pptx (the original, included in this repo)
fidelity: the summary above the divider was rewritten from the deck on 2026-09-20 after an audit found errors in the earlier version; it is still a condensed paraphrase and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 20 embedded image(s); code shown in images is transcribed in the summary)
source-mtime: 2025-11-03T10:04:32
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Module 04: Process Synchronization

Lecture slides (42) on process synchronization: the shared-`counter` race condition, the critical-section problem and its three requirements, hardware solutions (`test_and_set`, `compare_and_swap`), mutex locks, semaphores (counting, binary, blocking implementation) and the classic problems (bounded buffer, readers–writers, dining philosophers) with the four deadlock conditions. **Peterson's solution is not in this deck.**
Pptx last modified: 03-11-2025.
Rewritten 2026-09-20 directly from the deck: an audit found the earlier summary had wrong slide numbers, a Peterson's-solution section that is not in the deck, and textbook additions (`spinlock`, `proberen`/`verhogen`, `empty`/`full` names). Slide numbers below are the deck's real ones. Code is transcribed from the slide images and was checked against renders of slides 4, 11, 12, 13, 15 and 38; slides 40 (a meme) and 35 (a shared-file diagram) carry no further information.

## Content

### Slides 1–5: Background, producer–consumer `counter`, race condition
- Processes can execute concurrently or in parallel; the CPU scheduler switches rapidly between them; a process may be interrupted at any point; parallel execution means two instruction streams on separate cores. Concurrent access to shared data can break its integrity.
- **Slide 3, Producer–Consumer:** add an integer `counter`, initialised to 0, incremented when an item is added to the buffer and decremented when one is removed (the code is in images).
- **Slide 4, Data integrity problem:** `counter++` compiles to `register1 = counter; register1 = register1 + 1; counter = register1`, and `counter--` to `register2 = counter; register2 = register2 - 1; counter = register2` (registers are local CPU registers). Interleaved trace on the slide: T0 producer `register1 = counter` {register1 = 5}; T1 producer `register1 = register1 + 1` {6}; T2 consumer `register2 = counter` {register2 = 5}; T3 consumer `register2 = register2 - 1` {4}; T4 producer `counter = register1` {counter = 6}; T5 consumer `counter = register2` {counter = 4} — an incorrect state.
- **Slide 5, Race condition:** several processes access and manipulate the same data concurrently, and the outcome depends on the order of access. To guard against it, only one process at a time may manipulate the shared counter; the processes must be synchronized.

### Slides 6–8: The critical-section problem
- Slide 6 is a section divider. **Slide 7:** n processes {P0, P1, …, Pn−1}. *Critical section:* the segment of code of each process that may change common variables, update a table, write a file, and so on; while one process executes its critical section no other may execute its own. *Entry section:* code implementing the request to enter. *Exit section:* code exiting the critical section. *Remainder section:* the rest of the program. (The slide lists these as bullets; no code structure is shown.)
- **Slide 8, three requirements:**
  1. **Mutual exclusion:** if a process is executing its critical section, no other process can be executing in theirs.
  2. **Progress:** if no process is in its critical section and some wish to enter, only those not executing in their remainder section can participate in deciding which enters next, and this selection cannot be postponed indefinitely.
  3. **Bounded waiting:** there is a bound on the number of times other processes can enter their critical sections after a process has requested entry and before that request is granted.

### Slides 9–12: Hardware-based solutions
- Slide 9 is a divider. **Slide 10:** solutions range from hardware to software APIs and rest on *locking*; on a single processor the problem can be solved by preventing interrupts while a shared variable is modified; multiprocessors need other measures; modern systems provide atomic instructions to test and modify a word or swap two words: `test_and_set()` and `compare_and_swap()`.
- **Slide 11, `test_and_set()`** (executed atomically; mutual exclusion by initialising a Boolean `lock` to false):
  ```c
  boolean test_and_set(boolean *target) {
      boolean rv = *target;
      *target = true;
      return rv;
  }
  do {
      while (test_and_set(&lock))
          ; /* do nothing */
      /* critical section */
      lock = false;
      /* remainder section */
  } while (true);
  ```
- **Slide 12, `compare_and_swap()`** (global `lock` initialised to 0; the first process to invoke it sets `lock` to 1 and no other can execute its critical section until this one sets it back to 0):
  ```c
  int compare_and_swap(int *value, int expected, int new_value) {
      int temp = *value;
      if (*value == expected)
          *value = new_value;
      return temp;
  }
  do {
      while (compare_and_swap(&lock, 0, 1) != 0)
          ; /* do nothing */
      /* critical section */
      lock = 0;
      /* remainder section */
  } while (true);
  ```

### Slides 13–14: Mutex locks
- **Slide 13:** OS designers build software tools to solve the critical-section problem; the simplest is the mutex lock (mutex = mutual exclusion). A process must `acquire()` the lock before entering and `release()` it after exiting; the lock has a Boolean `available` telling whether it is free (Figure 5.8 on the slide):
  ```c
  acquire() {
      while (!available)
          ; /* busy wait */
      available = false;
  }
  release() {
      available = true;
  }
  ```
  (loop structure: `do { acquire lock; critical section; release lock; remainder section } while (true);`)
- **Slide 14, Mutex implementation** (declare, initialise, destroy the mutex; call acquire/release), pseudocode: shared `int sum = 0; mutex m;` two threads `t1`, `t2` (ids 1, 2); `main` runs `mutex_initialize(m, NULL)`, `thread_create` ×2, `thread_join` ×2, `mutex_destroy(m)`, prints "Total count:"; `func_thread(id)` prints "Entered in Thread", then loops `i = 0 to 2` doing `mutex_lock(m); sum = sum + 1; mutex_unlock(m)`.

### Slides 15–24: Semaphores
- **Slide 15:** a semaphore `S` is an integer variable accessed only through the atomic operations `wait()` and `signal()`; when one process modifies the value no other may modify it at the same time; in `wait(S)` the test (`S <= 0`) and the possible modification (`S--`) must run without interruption. Definitions (busy-wait form):
  ```c
  wait(S) {
      while (S <= 0)
          ; // busy wait
      S--;
  }
  signal(S) {
      S++;
  }
  ```
- **Slides 16–17, types:** *counting* semaphore — value ranges over an unrestricted domain, controls access to a resource with a finite number of instances; initialised to the number of resources (`S = n`); each user does `wait()` (`S = S - 1`), each release does `signal()` (`S = S + 1`); at `S = 0` all resources are in use and further requesters block until `S > 0`. *Binary* semaphore — value only 0 or 1, behaves like a mutex lock.
- **Slide 18, ordering with a binary semaphore:** P1 has statement S1, P2 has S2, S1 must run before S2; semaphore `sync` initialised to 0; P1: `S1; signal(sync);` P2: `wait(sync); S2;`.
- **Slide 19, mutual exclusion:** a semaphore `mutex` shared by n processes, initialised to 1; each process: `do { wait(mutex); //critical section  signal(mutex); //remainder section } while (TRUE);`
- **Slides 20–23, implementation:** slide 20 is a divider. Slide 21: the implementation must guarantee no two processes execute `wait()`/`signal()` on the same semaphore at once, so the implementation is itself a critical-section problem; busy waiting inside it is tolerable because the code is short (little busy waiting if the critical section is rarely occupied), but applications may spend long times in critical sections, so busy waiting there is not a good solution. Slide 22: instead of busy waiting, a process finding the value not positive blocks itself into a waiting queue tied to the semaphore (state → waiting, control goes to the CPU scheduler) and is restarted by a `wakeup()` that moves it from waiting to ready when another process calls `signal()`. Slide 23 definitions:
  ```c
  typedef struct { int value; struct process *list; } semaphore;
  wait(semaphore *S) {
      S->value--;
      if (S->value < 0) {
          add this process to S->list;
          block();
      }
  }
  signal(semaphore *S) {
      S->value++;
      if (S->value <= 0) {
          remove a process P from S->list;
          wakeup(P);
      }
  }
  ```
- **Slide 24:** same threaded-sum pseudocode as slide 14 but with `semaphore s`, `semaphore_initialize(s, 0, 1)`, `semaphore_wait(s)` / `semaphore_signal(s)` around `sum = sum + 1`, and `semaphore_destroy(s)`.

### Slides 25–30: Bounded-buffer problem
- Producer and consumer share a fixed-size buffer used as a queue; concern 1: the producer must not produce when the buffer is full; concern 2: the consumer must not remove when it is empty.
- **Slide 27, inadequate solution** (`itemCount`, `Sleep()`, `wakeup()`): producer sleeps when `itemCount == BufferSize`, consumer sleeps when `itemCount == 0`; each wakes the other on the 1 / `BufferSize - 1` transitions. It can deadlock: if the consumer is interrupted after checking `itemCount` and before `sleep()`, the producer's `wakeup()` is lost; the producer then fills the buffer and sleeps, and the consumer sleeps forever.
- **Slide 28, deadlock and starvation:** two or more processes waiting indefinitely for an event that only the others can cause (P0: `wait(S); wait(Q); … signal(S); signal(Q);` P1: `wait(Q); wait(S); … signal(Q); signal(S);`). Starvation: processes wait indefinitely within the semaphore, e.g. if the list is served in LIFO order.
- **Slides 29–30, solution:** two semaphores solve the lost-wakeup problem — `countFill` (items available to read) and `countEmpty` (space available to write); a producer decrementing `countEmpty` at 0 sleeps until a consume increments it. Code (slide 30): `semaphore mutex = 1; semaphore countFull = 0; semaphore countEmpty = BUFFER_SIZE;` producer: `item = producedItem(); wait(countEmpty); wait(mutex); putIntoBuffer(item); signal(mutex); signal(countFull);` consumer: `wait(countFull); wait(mutex); item = removeFromBuffer(); signal(mutex); signal(countEmpty); consumeItem(item);`

### Slides 31–35: Readers–writers problem
- Data shared among processes; any number of readers may read simultaneously, only one writer may write, and while a writer writes no reader may read. *First variation:* no reader waits unless a writer already has permission (writers may starve). *Second variation:* once a writer is ready it goes as soon as possible, so no new readers start (readers may starve).
- **Slides 33–35, first-variation solution:** `semaphore rw_mutex = 1; semaphore mutex = 1; int read_count = 0;` `rw_mutex` is common to readers and writers and gives writers mutual exclusion; `mutex` protects `read_count`; `read_count` counts current readers. If a writer is in the critical section and n readers wait, one queues on `rw_mutex` and n−1 on `mutex`; a writer's `signal(rw_mutex)` resumes either the waiting readers or one waiting writer. Reader: `wait(mutex); read_count++; if (read_count == 1) wait(rw_mutex); signal(mutex); /* reading */ wait(mutex); read_count--; if (read_count == 0) signal(rw_mutex); signal(mutex);` Writer: `wait(rw_mutex); /* writing */ signal(rw_mutex);`

### Slides 36–41: Dining philosophers
- Five philosophers, a bowl of rice in the centre, five single chopsticks; a hungry philosopher picks up the two nearest chopsticks and eats only with both, then puts both down and thinks.
- **Slide 38, simple solution:** `semaphore chopstick[5];` philosopher i: `do { wait(chopstick[i]); wait(chopstick[(i+1) % 5]); /* eat for awhile */ signal(chopstick[i]); signal(chopstick[(i+1) % 5]); /* think for awhile */ } while (true);`
- **Slide 39:** if all philosophers get hungry at once and each takes the left chopstick, none finds a second one and all starve (deadlock).
- **Slide 41, prevention:** (1) allow at most four philosophers at the table; (2) allow a philosopher to pick up chopsticks only if both are available (picked up inside a critical section); (3) asymmetric solution — odd-numbered philosophers take left then right, even-numbered take right then left. (Slide 40 is a meme image.)

### Slide 42: Necessary conditions for deadlock
- Mutual exclusion; hold and wait; no preemption (a resource cannot be forcibly released); circular wait. If one is absent the system is deadlock-free.

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets; list numbering, formatting and images are not reproduced -->

<!-- slide 1 -->
### OPERATING SYSTEMS Process Synchronization
FBA

<!-- slide 2 -->
### BACKGROUND
Processes can execute concurrently or in parallel
CPU scheduler switches rapidly between processes to provide concurrent execution
A process may be interrupted at any point in its instruction stream
Parallel execution, in which two instruction streams execute simultaneously on separate processing cores
We will explain how concurrent or parallel execution can contribute to issues involving the integrity of data shared by several processes

<!-- slide 3 -->
### PRODUCER–CONSUMER PROBLEM
Modify the algorithm to remedy this deficiency - add an integer variable counter, initialized to 0
counter is incremented every time we add a new item to the buffer
decremented every time we remove one item from the buffer
[image]
[image]

<!-- slide 4 -->
### DATA INTEGRITY PROBLEM
“counter++” and “counter-- “ in machine language is like in the above.
register1 and register2 is local CPU registers.
Concurrent execution of “counter++” and “counter--” and allowing them to manipulate the counter variable create incorrect state.
[image]
[image]
[image]

<!-- slide 5 -->
### RACE CONDITION
Several process access and manipulate the same data concurrently
Outcome of the execution depends on the particular order in which the access takes place
To guard against this condition –
  Ensure that only one process at a time can manipulate the counter variable (shared data)
  The processes should be synchronized

<!-- slide 6 -->
### OPERATING SYSTEMS Critical Section
FBA

<!-- slide 7 -->
### CRITICAL SECTION
Consider a system consisting of n processes {P0, P1, ... , Pn−1}.
Critical Section: segment of code of each process, which may change common variables, update a table, write a file and so on.
While one process execute its critical section, no other process can execute their own critical section.
Entry Section: section of code implementing critical section execution request
Exit Section: section of code exiting from critical section
Remainder section: Remaining code of the program.

<!-- slide 8 -->
### REQUIREMENTS OF SOLUTION TO CRITICAL SECTION PROBLEM
Mutual exclusion:
  If a process is executing its critical section, no other process can be executing in their critical sections.
Progress:
  No process is executing in its critical section
  Some process wish to enter their critical sections
  Only those, who are not executing in their remainder section can participate in deciding which will enter the CS.
  This selection cannot be postponed indefinitely.
Bounded waiting:
  Bound or Limit on number of times other process can enter their CS after a process has made request to enter its CS and the request is granted

<!-- slide 9 -->
### OPERATING SYSTEMS Hardware based solution for Critical Section Problem
FBA

<!-- slide 10 -->
### HARDWARE-BASED SOLUTION TO THE CRITICAL SECTION PROBLEM
More solutions to the critical-section problem using techniques ranging from hardware to software-based APIs
These solutions are based on the premise of locking — protecting critical regions through the use of locks.
In a single-processor environment CS problem can be solved by preventing interrupts from occurring while a shared variable is being modified.
For multiprocessor environment, we need different measures.
Modern computer systems allow to test and modify the content of a word or to swap the contents of two words atomically – which is uninterruptable unit. We can use test_and_set() and compare_and_swap() instructions.

<!-- slide 11 -->
### TEST_AND_SET( )
Executed atomically
Mutual exclusion can be implemented by initializing a Boolean variable lock to false
[image]
[image]

<!-- slide 12 -->
### COMPARE_AND_SWAP( )
[image]
Mutual exclusion can be achieved by declaring a global variable lock and initializing it to 0
First process that invokes this instruction will set lock to 1 and no other process can execute CS until this process updates it to 0 after CS execution.
[image]

<!-- slide 13 -->
### MUTEX LOCKS
[image]
[image]
[image]
Operating-systems designers build software tools to solve CS problem.
Simplest of these tools is “Mutex Lock” ( Mutex = Mutual Exclusion)
A process must acquire the lock before entering CS [ acquire() function ]
A process must release the lock after exiting the CS [ release() function ]
Mutex lock has a variable, available which indicates if the lock is available.

<!-- slide 14 -->
### MUTEX IMPLEMENTATION
Declaring mutex variable
Initializing the mutex
Destroying the mutex after usage
Calling acquire func.
Calling release func.
[image]
Output:
int sum = 0
mutex m
int id1 = 1
int id2 = 2
thread t1
thread t2
main:
mutex_initialize(m, NULL)
thread_create(t1, func_thread, id1)
thread_create(t2, func_thread, id2)
thread_join(t1, NULL)
thread_join(t2, NULL)
mutex_destroy(m)
print("Total count:", sum)
return
func_thread(id):
print("Entered in Thread", id)
for(i = 0 to 2):
mutex_lock(m)
sum = sum + 1
mutex_unlock(m)
return

<!-- slide 15 -->
### SEMAPHORE
A semaphore S is an integer variable
Is accessed only through two standard atomic operations: wait() and signal().
When one process modifies the semaphore value, no other process can simultaneously modify that same semaphore value.
In case of wait(S), the testing of the integer value of S (S ≤ 0), as well as its possible modification (S--), must be executed without interruption, i.e., this operations are atomic.
[image]
[image]

<!-- slide 16 -->
### Types of Semaphores
Counting Semaphore: The value can range over an unrestricted domain.
  Used to control access to a given resource consisting of finite number of instances
  Solves various synchronization problems.
Binary Semaphore: The value can range only between 0 and 1. This behaves similar to Mutex Lock.

<!-- slide 17 -->
### Counting Semaphore
initialized to the number of resources available, S = n
Each process that wishes to use a resource performs a wait() operation
S = S -1
When a process releases a resource, it performs a signal() operation
S = S +1
When S becomes 0, all resources are being used
processes that wish to use a resource will block until S>0

<!-- slide 18 -->
### Binary Semaphore - Synchronization
P1 has statement S1
P2 has statement S2
We want to make sure that S1 executes before S2
We can use a semaphore variable sync and initialize it to 0
P1:
S1;
signal(sync);
P2:
wait(sync);
S2;

<!-- slide 19 -->
### Mutual Exclusion With Semaphores
Binary Semaphores (mutex) can be used to solve CS problem.
A semaphore variable (say mutex) can be shared by n processes and initialized to 1.
Each process is structured as follows :
  do{
  wait (mutex);
  //critical section
  signal(mutex);
  //remainder section
  }while (TRUE);

<!-- slide 20 -->
### OPERATING SYSTEMS Semaphore Implementation
FBA

<!-- slide 21 -->
### SEMAPHORE IMPLEMENTATION
Must guarantee that no two processes can execute the wait() and signal() on the same semaphore at the same time
Thus, the implementation becomes the critical section problem where the wait and signal code are placed in the critical section
Could now have busy waiting in critical section implementation
    But implementation code is short
    Little busy waiting if critical section rarely occupied
Note that applications may spend lots of time in critical sections and therefore this is not a good solution

<!-- slide 22 -->
### SEMAPHORE IMPLEMENTATION
When a process executes the wait() operation and finds that the semaphore value is not positive, it must wait
Rather than this busy waiting, the process can block itself which places it into a waiting queue associated with the semaphore
State of the process is switched to the waiting state and control is transferred to CPU scheduler which selects another process to execute.
It will be restarted when some other process executes a signal() operation
Restarted by a wakeup() operation that changes it from waiting state to ready state.

<!-- slide 23 -->
### SEMAPHORE IMPLEMENTATION
Definition of a semaphore:
typedef struct {
int value;
struct process *list;
} semaphore;
Definition of wait():
wait(semaphore *S) {
S->value--;
if (S->value < 0) {
add this process to S->list;
block();
}
}
Definition of signal():
signal(semaphore *S) {
S->value++;
if (S->value <= 0) {
remove a process P from S->list;
wakeup(P);
}
}

<!-- slide 24 -->
### SEMAPHORE IMPLEMENTATION
Declaring semaphore variable
Initializing the semaphore
Destroying the semaphore after usage
Calling wait func.
Calling signal func.
[image]
Output:
int sum = 0
semaphore s
int id1 = 1
int id2 = 2
thread t1
thread t2
main:
semaphore_initialize(s, 0, 1)
thread_create(t1, func_thread, id1)
thread_create(t2, func_thread, id2)
thread_join(t1, NULL)
thread_join(t2, NULL)
semaphore_destroy(s)
print("Total count:", sum)
return
func_thread(id):
print("Entered in Thread", id)
for(i = 0 to 2):
semaphore_wait(s)
sum = sum + 1
semaphore_signal(s)
return

<!-- slide 25 -->
### OPERATING SYSTEMS (Application of Semaphore) Bounded Buffer Problem

<!-- slide 26 -->
### Bounded Buffer Problem
Producer and Consumer share a fixed size buffer used as a queue
Producer generates piece of data and put it into the buffer and start again
Consumer removes data from the buffer
Concern 1: Producer won’t produce data when the buffer is full
Concern 2: Consumer won’t remove data when the buffer is empty

<!-- slide 27 -->
### Inadequate Solution
procedure producer(){
while(true){
item = produceItem();
if(itemCount == BufferSize)
Sleep();
putIntoBuffer(item);
itemCount ++;
if(itemCount == 1)
wakeup(consumer);
}
}
procedure consumer(){
while(true){
if(itemCount == 0)
Sleep();
item = removeFromBuffer();
itemCount --;
if(itemCount == BufferSize - 1)
wakeup(producer);
}
}
DEADLOCK !!!
If, consumer is interrupted just after checking the itemCount value and before sleep() call, and producer produces an item it will call wakeup(). But, this call will be lost as no consumer in sleeping yet. And it will continue to produce items. Eventually, producer will sleep when the buffer is full. On the other hand, consumer will also stay in sleep as producer won’t call wakeup().

<!-- slide 28 -->
### Deadlock & Starvation
Two or more process can wait indefinitely for an event - DEADLOCK !!!
It occurs because - two process depends on each other for causing an event in a specific manner
Starvation: Processes wait indefinitely within the semaphore
Occurs if we remove processes from the list associated with a semaphore in LIFO (last-in, first-out) order
P0
wait(S);
wait(Q);
. .
. .
. .
signal(S);
signal(Q);
P1
wait(Q);
wait(S);
. .
. .
. .
signal(Q);
signal(S);

<!-- slide 29 -->
### Use of Semaphore for Solution
Solves the lost wakeup call by using two semaphore - countFill, countEmpty
countFill: number of available item in the buffer to be read
countEmpty: number of available space in the buffer to write
If producer tries to decrement countEmpty when it is 0, will be put to sleep
When an item is consumed, countEmpty will be incremented and producer wakes up.
Consumer works analogously.

<!-- slide 30 -->
### Semaphore for multiple producer consumer
semaphore mutex = 1;
semaphore countFull = 0;
semaphore countEmpty = BUFFER_SIZE;
procedure producer(){
while(true){
item = producedItem();
wait(countEmpty);
wait(mutex);
putIntoBuffer(item);
signal(mutex);
signal(countFull);
}
}
procedure consumer(){
while(true){
wait(countFull);
wait(mutex);
item = removeFromBuffer();
signal(mutex);
signal(countEmpty);
consumeItem(item);
}
}

<!-- slide 31 -->
### OPERATING SYSTEMS (Application of Semaphore) Reader-Writer Problem

<!-- slide 32 -->
### Reader - Writer Problem
Data is shared among number of processes
Any number of readers may simultaneously read the data
Only one writer can write to the data
If a writer is writing, no reader can read
If at least one reader is reading, no writer can write to it
Readers only read, Writers only write
Variation of the problem:
First Variation: No reader will be kept waiting, unless a writer has got the permission to use the data, i.e., no reader should wait for other reader to finish
Second Variation: Once a writer is ready, the writer performs as soon as possible, i.e., no new readers can start reading.
Solution to either problem may create starvation -
  First case: writer may starve
  Second case: reader may starve

<!-- slide 33 -->
### Solution to First Variation
Reader process has the following data structures -
semaphore rw_mutex = 1;
semaphore mutex = 1;
int read count = 0;
rw_mutex: common to both reader and writer, functions as mutual exclusion semaphore for writer
mutex: ensures mutual exclusion when read_count is updated
read_count: keeps track of how many processes are reading the object

<!-- slide 34 -->
### Solution to First Variation
If a writer is in the critical section and n readers are waiting
  One reader is queued on rw_mutex
  Other n-1 readers are queued on mutex
When a writer executes signal(rw_mutex)
  Resume the execution of either waiting readers or single waiting writer
Multiple process can acquire rw_mutex in read mode
Only one process can acquire rw_mutex in write mode
do{
wait( mutex );
read_count++;
if( read_count == 1)
wait( rw_mutex );
signal( mutex );
/* reading is performed */
wait( mutex );
read_count--;
if (read_count == 0)
signal( rw_mutex );
signal( mutex );
}while(true)
do {
wait( rw_mutex );
/* writing is performed */
signal( rw_mutex );
} while(true);
Writers
Readers

<!-- slide 35 -->
rw_mutex:
mutex:
read_count:
[image: All Documents]
Shared File
do{
wait( mutex );
read_count++;
if( read_count == 1)
wait( rw_mutex );
signal( mutex );
/* reading is performed */
wait( mutex );
read_count--;
if (read_count == 0)
signal( rw_mutex );
signal( mutex );
}while(true)
do {
wait( rw_mutex );
/* writing is performed */
signal( rw_mutex );
} while(true);
Writer
Readers

<!-- slide 36 -->
### OPERATING SYSTEMS (Application of Semaphore) Dining Philosophers Problem

<!-- slide 37 -->
### Dining-Philosophers Problem
[image]
In the center of the table is a bowl of rice, and the table is laid with five single chopsticks
From time to time, a philosopher gets hungry and tries to pick up the two chopsticks that are closest to her
When a hungry philosopher has both her chopsticks at the same time, she eats without releasing the chopsticks
When she is finished eating, she puts down both chopsticks and starts thinking again

<!-- slide 38 -->
### Solution to the problem
One of the simple solution is to use 5 semaphores for 5 chopsticks.
semaphore chopstick[5];
Structure of ith philosopher’s process can be -
[image]
[image]
0
1
2
3
4
0
1
2
3
4

<!-- slide 39 -->
### BUT GUESS WHAT’S COMING????
[image]
If all the philosophers are hungry at the same time and grab their left chopstick by calling wait(chopstick[i]), No one will ever find a second chopstick for eating. Consequently, they will die from starvation :P
[image]
0
1
2
3
4
0
1
2
3
4

<!-- slide 40 -->
### How to prevent deadlock in this solution?
[image]

<!-- slide 41 -->
### How to prevent deadlock in this solution?
Allow at most four philosophers to be sitting simultaneously at the table
Allow a philosopher to pick up her chopsticks only if both chopsticks are available (to do this, she must pick them up in a critical section).
Use an asymmetric solution — that is, an odd-numbered philosopher picks up first her left chopstick and then her right chopstick, whereas an even-numbered philosopher picks up her right chopstick and then her left chopstick.

<!-- slide 42 -->
### Necessary Conditions for Deadlock
Necessary conditions for deadlocks are -
Mutual Exclusion
Hold and Wait
No Preemption (cannot force to release the resource)
Circular wait
If one of these are not present in a system, then the system is “Deadlock Free”.
