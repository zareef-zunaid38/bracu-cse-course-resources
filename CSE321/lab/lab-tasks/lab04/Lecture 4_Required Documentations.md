---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# Lecture 4: Inter-Process Communication (IPC) Documentation

Detailed API reference and procedural guide for Unix IPC mechanisms in C: pipes (`pipe`, `read`, `write`, `close`), System V shared memory (`ftok`, `shmget`, `shmat`, `shmctl`), and message queues (`msgget`, `msgsnd`, `msgrcv`, `msgctl`).
Docx last modified: 20-07-2026.

## Content

### Overview of IPC
Inter-Process Communication (IPC) provides mechanisms for processes running in distinct address spaces to exchange data and synchronize actions at runtime via pipes, shared memory, and message passing.

---

### 1. Pipes
- **Header**: `#include <unistd.h>`
- **Concept**: Unidirectional byte stream connecting two related processes. Has read end `pipedes[0]` and write end `pipedes[1]`. For bidirectional communication, two distinct pipes are required (`pipe1` and `pipe2`).
- **System Calls**:
  - `int pipe(int pipedes[2]);`
    - Creates pipe. `pipedes[0]` = read end, `pipedes[1]` = write end. Returns `0` on success, `-1` on error.
  - `int close(int fd);`
    - Closes file descriptor. Returns `0` on success, `-1` on error.
  - `ssize_t write(int fd, void *buf, size_t count);`
    - Writes `count` bytes from `buf` to `fd`. Returns bytes written or `-1` on failure.
  - `ssize_t read(int fd, void *buf, size_t count);`
    - Reads up to `count` bytes from `fd` into `buf`. Returns bytes read (`0` at EOF) or `-1` on failure.

---

### 2. Shared Memory
- **Headers**:
  ```c
  #include <sys/types.h>
  #include <sys/ipc.h>
  #include <sys/shm.h>
  ```
- **Procedures**:
  1. **Key Generation (`key_t`)**:
     - Explicit: `key_t key = 1234;`
     - File-to-key conversion: `key_t key = ftok("./", 'x');`
     - Private key: `IPC_PRIVATE`
  2. **Allocating Shared Memory (`shmget`)**:
     ```c
     int shm_id = shmget(key_t key, size_t size, int flag);
     ```
     - Flags: `IPC_CREAT | 0666` (create with read/write permissions for user, group, others) or `0666` (access existing).
     - Returns shared memory ID (`shm_id`) or `-1` on error.
  3. **Attaching Address Space (`shmat`)**:
     ```c
     void *shm_ptr = shmat(int shm_id, const void *shmaddr, int shmflg);
     ```
     - Pass `NULL` and `0` for default system-allocated address space. Returns pointer to attached segment or `(void *)-1`.
  4. **Detaching & Removing Segment (`shmctl`)**:
     ```c
     shmctl(shm_id, IPC_RMID, NULL);
     ```
     - Deletes shared memory segment after all cooperating processes finish.

---

### 3. Message Passing & Message Queues
- **Headers**:
  ```c
  #include <sys/types.h>
  #include <sys/ipc.h>
  #include <sys/msg.h>
  ```
- **Message Structure**:
  ```c
  struct msgbuf {
      long mtype;       /* message type, must be > 0 */
      char mtext[100];  /* message data payload */
  };
  ```
- **System Calls**:
  1. **Creating/Accessing Queue (`msgget`)**:
     ```c
     int msgget(key_t key, int msgflg);
     ```
     - Flag: `IPC_CREAT | 0666`. Returns message queue identifier `msgid` or `-1`.
  2. **Sending Message (`msgsnd`)**:
     ```c
     int msgsnd(int msgid, const void *msgp, size_t msgsz, int msgflg);
     ```
     - `msgsz`: size of payload (`sizeof(msg) - sizeof(long)`). `msgflg`: `0` (blocking) or `IPC_NOWAIT`. Returns `0` on success, `-1` on error.
  3. **Receiving Message (`msgrcv`)**:
     ```c
     ssize_t msgrcv(int msgid, void *msgp, size_t msgsz, long msgtype, int msgflg);
     ```
     - `msgtype`:
       - `0`: Reads first message in queue.
       - `> 0`: Reads first message matching exact type `msgtype`.
       - `< 0`: Reads first message with lowest type $\le |	ext{msgtype}|$.
     - Flags: `0`, `IPC_NOWAIT`, `MSG_EXCEPT`, `MSG_NOERROR` (truncates over-sized payload instead of returning `E2BIG`).
  4. **Removing Queue (`msgctl`)**:
     ```c
     int msgctl(int msgid, IPC_RMID, NULL);
     ```
