---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# Lecture 3.2: Process & Thread Synchronization (Semaphores & Mutexes)

Reference guide for POSIX semaphores (`semaphore.h`) and mutexes (`pthread.h`), including operations, memory visibility, function signatures (`sem_init`, `sem_wait`, `sem_post`, `sem_destroy`, `pthread_mutex_*`), and usage rules.
Docx last modified: 13-07-2026.

## Content

### Overview & Compilation
- Synchronization mechanisms in POSIX C prevent race conditions in concurrent multi-threaded programs.
- **Compilation**: Compile with `-pthread` (or `-lpthread`).

---

### Semaphores in Linux
- **Header**: `#include <semaphore.h>`
- **Concept**: A non-negative integer variable $S \ge 0$ supporting atomic operations:
  - `wait` (decrement / block if 0)
  - `post` (increment / wake blocked thread)
- **Counted Semaphore**: Manages access to $N$ interchangeable resource instances.
- **Binary Semaphore**: Restricted to values 0 and 1.
- **Declaration**: `sem_t sem;`

#### Semaphore Functions:
1. `int sem_init(sem_t *sem, int pshared, unsigned int value);`
   - Initializes semaphore `*sem` with initial value `value`.
   - `pshared == 0`: Shared between threads of the calling process (stored in global/heap memory).
   - `pshared != 0`: Shared between processes (must be located in shared memory).
   - Return: `0` on success, `-1` on failure with `errno` set. Re-initializing an existing semaphore is undefined.
2. `int sem_wait(sem_t *sem);`
   - Decrements semaphore. If value is 0, blocks calling thread until value > 0.
   - Related: `sem_trywait()` (non-blocking), `sem_timedwait()` (timeout).
   - Return: `0` on success, `-1` on error.
3. `int sem_post(sem_t *sem);`
   - Increments semaphore. Wakes up a waiting thread if any.
   - Return: `0` on success, `-1` on error.
4. `int sem_destroy(sem_t *sem);`
   - Destroys unmanaged semaphore. Produces undefined behavior if threads are currently blocked on it.
   - Return: `0` on success, `-1` on error.

---

### Mutexes in Linux
- **Header**: `#include <pthread.h>`
- **Concept**: Mutual exclusion lock (binary semaphore with ownership semantics). Only the specific thread that locked the mutex is allowed to unlock it.
- **Declaration**: `pthread_mutex_t mutex;`

#### Mutex Functions:
1. `int pthread_mutex_init(pthread_mutex_t *restrict mutex, const pthread_mutexattr_t *restrict attr);`
   - Initializes `*mutex` with attributes `attr` (`NULL` for defaults) in "unlocked" state.
   - Return: `0` on success, non-zero error code on failure.
2. `int pthread_mutex_destroy(pthread_mutex_t *restrict mutex);`
   - Destroys initialized mutex. Safe only when mutex is in unlocked state.
   - Return: `0` on success, non-zero error code on failure.
3. `int pthread_mutex_lock(pthread_mutex_t *mutex);`
   - Locks `*mutex`. If already locked, blocks caller until unlocked.
   - Return: `0` on success, non-zero error code on failure.
4. `int pthread_mutex_unlock(pthread_mutex_t *mutex);`
   - Unlocks `*mutex`. Error if caller does not own the lock or mutex is already unlocked.
   - Return: `0` on success, non-zero error code on failure.
