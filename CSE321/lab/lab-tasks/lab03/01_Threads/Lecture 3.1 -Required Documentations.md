---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# Lecture 3.1: POSIX Threads (pthreads) Documentation

Reference guide for Linux POSIX Threads API (`pthread.h`), compilation with `-lpthread`, and core functions (`pthread_create`, `pthread_exit`, `pthread_join`).
Docx last modified: 13-07-2026.

## Content

### Overview: POSIX Threads in Linux
- The C standard API for thread programming on Linux platforms is **POSIX Threads** (`pthread`).
- Enables concurrent execution flows within a single process address space.
- Ideal for multi-core processors and SMP architectures to achieve parallel execution.

### Implementation & Compilation
- **Header File**: `#include <pthread.h>`
- **Compilation Command**:
  ```bash
  gcc -o th th.c -lpthread
  ./th
  ```

### Core pthread Functions

#### 1. `pthread_create`
```c
int pthread_create(pthread_t *thread_id, const pthread_attr_t *attr, void *(*thread_function)(void *), void *argument);
```
- **Purpose**: Creates a new thread within the calling process.
- **Parameters**:
  - `thread_id`: Pointer to `pthread_t` where the system writes the unique ID of the created thread.
  - `attr`: Pointer to `pthread_attr_t` structure defining thread attributes (scheduling policy, stack size/address). Pass `NULL` for defaults.
  - `thread_function`: Pointer to the start routine executed by the thread (takes `void *`, returns `void *`).
  - `argument`: Pointer to `void` passed as the single argument to `thread_function`.

#### 2. `pthread_exit`
```c
void pthread_exit(void *return_value);
```
- **Purpose**: Explicitly terminates the calling thread.
- **Parameters**:
  - `return_value`: Pointer storing the thread exit status made available to any joining thread. Must not point to local/stack memory of the exiting thread (use global or heap-allocated memory).

#### 3. `pthread_join`
```c
int pthread_join(pthread_t thread_id, void **thread_return);
```
- **Purpose**: Suspends execution of the calling thread until the target thread specified by `thread_id` terminates.
- **Parameters**:
  - `thread_id`: Thread ID of the target thread to wait for.
  - `thread_return`: Double pointer (`void **`) to the location where the target thread's exit status (`return_value` from `pthread_exit`) will be stored.
