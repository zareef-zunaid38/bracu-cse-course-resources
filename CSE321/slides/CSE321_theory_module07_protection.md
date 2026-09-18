# CSE321 Module 07: Protection — sidecar (read this, not the .pptx)

Lecture slides covering operating system protection goals, principle of least privilege, protection domains (UNIX setuid, Android App IDs), Access Matrix model, revocation methods, Mandatory vs Discretionary Access Control (MAC/DAC), capability systems, and sandboxing.
Pptx last modified: 03-09-2025.

## Content

### Slides 1–4: Protection Goals & Principles
- **Definition:** Protection refers to a mechanism for controlling access of programs, processes, or users to the resources defined by a computer system.
- **Goals:** Ensure that only authorized processes can access OS and user resources (hardware and software objects). Prevent accidental or malicious corruption.
- **Principle of Least Privilege:** Programs, users, and systems should be given just enough privileges to perform their tasks. Minimizes damage if an entity is compromised.

### Slides 5–7: Domain Structure & Domain Implementation
- **Need-to-know Principle:** A process should only access resources necessary to complete its current task.
- **Domain:** A set of access rights, where an **Access Right** is a pair $\langle \text{object-name}, \text{rights-set} \rangle$.
- **Domain Implementations:**
  - **UNIX:**
    - Domain = `user-id`.
    - Domain switch accomplished via file system `setuid` bit: when executed, process temporarily assumes UID of file owner.
    - Password domain switch via `su` or command prefix via `sudo`.
  - **Android Application IDs:**
    - Each installed app is assigned a unique UID and GID by `installd`.
    - Private directory `/data/data/<appname>` granted exclusively to that UID/GID.
    - Kernel modified to enforce GID-level network/socket permissions (e.g., AID_INET).
    - Isolated UIDs prevent unauthorized RPC service requests.

### Slides 8–13: The Access Matrix Model
- Conceptual matrix representing protection state:
  - **Rows:** Represent Domains ($D_i$).
  - **Columns:** Represent Objects ($O_j$).
  - **Entry $\text{Access}(i, j)$:** Set of operations that a process executing in Domain $D_i$ can invoke on Object $O_j$ (e.g., `read`, `write`, `execute`).
- **Access Control List (ACL):** Storing matrix by columns (for each object, list domains and their allowed operations).
- **Capability List:** Storing matrix by rows (for each domain, list objects and operations allowed on them; capability is an unforgeable ticket).
- **Dynamic Rights in Access Matrix:**
  - **Copy Right ($*$):** Process can copy an access right within the same column to another domain.
  - **Owner Right:** Domain owning an object can add or remove rights in that object's column.
  - **Control Right:** Domain $D_i$ with control right over domain $D_j$ can modify entries in row $D_j$.
  - **Switch Right:** Allows a process executing in domain $D_i$ to switch to domain $D_j$.
- **Separation of Policy and Mechanism:** Mechanism (OS data structures and rules enforcement) is separated from Policy (user deciding who gets access to what).

### Slide 14: Revocation of Access Rights
- **Access-List Scheme:** Simple — locate right in object's ACL and delete it. Immediate, selective or general, partial or total, temporary or permanent.
- **Capability-List Scheme:** Difficult because capabilities are distributed across domain rows:
  - *Reacquisition:* Capabilities checked periodically or expire.
  - *Back-pointers:* Pointers from objects to all capabilities (expensive, used in MULTICS).
  - *Indirection:* Capability points to an entry in a global table; invalidating table entry revokes access (affects all domains holding capability).
  - *Keys:* Unique key in capability matched with lock on object; changing lock revokes access.

### Slide 15: Mandatory (MAC) vs Discretionary Access Control (DAC)
- **Discretionary Access Control (DAC):** Resource owner specifies who can access it (e.g., standard UNIX file permissions, Windows ACLs). Vulnerable if owner misconfigures permissions.
- **Mandatory Access Control (MAC):** System-wide security policy dictates access; neither users nor root can circumvent policy. Resources and processes assigned security labels (e.g., SELinux, TrustedBSD, macOS).

### Slides 16–17: Capability-Based Systems & POSIX.1e
- Slices root powers into distinct discrete capabilities represented by bitmap bits (e.g., `CAP_NET_ADMIN`, `CAP_SYS_ADMIN`).
- Allows fine-grained privilege assignment without granting full root powers.
- Bitmaps: *Permitted*, *Effective*, and *Inheritable*.
- Direct realization of Principle of Least Privilege: processes drop unneeded capabilities during execution.

### Slides 18–19: Other Modern Protection Improvements
- **System Integrity Protection (SIP):** Introduced in macOS; restricts access to system files, kernel extensions, and debugging even for root user.
- **System-Call Filtering:** Restricts system calls executable by a process (e.g., Linux `SECCOMP-BPF`).
- **Sandboxing:** Running processes in constrained environments with irremovable restrictions imposed at startup before `main()` (e.g., Apple Seatbelt, JVM sandboxes).
- **Language-Based Protection:** High-level language type safety and access control (Java, C#) enforced by compiler and runtime VM.
