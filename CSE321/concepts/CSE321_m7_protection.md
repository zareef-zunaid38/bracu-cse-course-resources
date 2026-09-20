---
type: module-notes
course: CSE321
module: 7
topics: "29–30"
title: Protection
status: draft
date: 2026-07-31
exam: final
---

# CSE321 — Module 7: Protection
**Topics 29–30 | Exam: Final | Silberschatz Ch.17 (17.1, 17.2, 17.4, 17.5, 17.7, 17.9, 17.10, 17.11) | No practice sheet — conceptual/theoretical only**

---

## Topic 29 — Protection Goals, Principles, and Access Matrix
**Ref: Silberschatz 17.1, 17.2, 17.4, 17.5**

### Goals of Protection
- Ensure that each program component operating within a system uses resources only in ways consistent with the policies for those resources.
- Prevent both **accidental** misuse (a buggy program overstepping its intended access) and **malicious** misuse (a program deliberately overstepping).
- Improve reliability by catching latent errors at interfaces between subsystems — a well-defined protection boundary turns an undetected error into an immediate, visible failure.

> **Distinction from Security (Module 8):** Protection is an **internal** mechanism problem — controlling access of already-authenticated processes/users to system resources according to policy. Security (next module) is the broader problem of defending the system from **external** threats. Protection mechanisms are one of the tools security policy relies on.

### Mechanism vs. Policy
A distinction the slides lead with, right after the Principle of Least Privilege:
- **Mechanism** — *what the OS actually builds in* to make protection possible (e.g., the access matrix machinery itself, the hardware checks).
- **Policy** — *what states are authorized or not*, i.e. the actual rules governing who can do what. Set by users/administrators, not hard-coded into the OS.

> **Exam note:** this separation is *why* the access matrix (below) is described as separating mechanism from policy — the OS provides the matrix machinery and enforces that only authorized agents can change it (mechanism); the user decides who gets what rights (policy).

### Domain of Protection
A process operates within a **protection domain**, which defines what resources it may access and how.

- A domain = a set of **(object, rights-set)** pairs. Each pair specifies an object and the subset of operations permitted on it. This is the same thing as an **access right**: `⟨object-name, rights-set⟩`.
- **Domain switching:** a process can be allowed to switch between domains during execution — e.g., a user-mode domain and a kernel-mode domain, switched via a system call. Domain-object associations can be **static** (fixed) or **dynamic** (change during execution, requiring domain switching).
- Domains can be realized as: a **user** (rights = whatever that user is permitted), a **process** (rights = whatever the process is permitted, independent of the user running it), or a **procedure** (local variables are within its own domain — data hiding).
- **Need-to-know principle:** a process should only be allowed to access resources actually necessary for the task it's currently performing, and only for which it's authorized — the domain-of-protection realization of least privilege.
- Protection domains are also visualized as concentric **rings of protection** — functions separated into domains and ordered hierarchically (e.g., kernel ring innermost, user applications outermost).

**Domain implementation in UNIX (concrete, slide-specific — likely testable):**
- Domain = **user-id**.
- Domain switch via the file system: each file has a **setuid bit**. When an executable with setuid on is run, the process's user-id is temporarily set to the file's *owner*, not the user who ran it — then reset when execution completes.
- Domain switch via passwords: the **`su`** command temporarily switches to another user's domain, given that user's password.
- Domain switch via commands: **`sudo`** executes a specified command in another domain, provided the original domain already has the privilege (or supplies the password).

**Domain implementation in Android (concrete, slide-specific):**
- Each installed app gets its own distinct **UID/GID**, assigned by the `installd` daemon, plus a private data directory (`/data/data/<appname>`) owned exclusively by that UID/GID — giving each app the same separation UNIX gives separate users.
- Extended via kernel modification: some operations (e.g., networking sockets) are restricted to members of a specific GID (e.g., `AID_INET`).
- **Isolated UIDs:** a further Android-specific restriction preventing a UID from initiating RPC requests to anything but a bare-minimum set of services.

### The Access Matrix
Formalizes the protection state of a system as a matrix:

```
                  Object₁      Object₂      Object₃      ...
Domain 1        { read }     { }          { read,write}
Domain 2        { }          { execute }  { read }
Domain 3        { read,write}{ read }     { }
```

- **Rows** = domains.
- **Columns** = objects (files, devices, other processes, even domains themselves — a domain can be an object of another domain's row, enabling controlled domain switching).
- **Entry [i, j]** = the set of operations a process executing in domain i may invoke on object j.

**Domain switching as a matrix operation:** if `switch` (Silberschatz's term — the slides call the same right `transfer`, know both names) is included as a right in `access(Di, Dj)`, a process executing in domain Di may switch to domain Dj.

**Four special access rights extend the model dynamically (not just static policy)** — the slides enumerate all four; my earlier draft only had two:
- **`copy`** (denoted `*`, applicable to objects) — allows a domain to copy one of its own access rights to another domain's entry for the same object. An asterisked right (`right*`) can itself be re-copied; without the asterisk, a copy is one-generation-only.
- **`owner`** (applicable to objects) — allows a domain to add or remove rights in *any* domain's entry for that object.
- **`control`** (applicable to domain objects, not ordinary objects) — allows a process executing in domain Di to modify the access rights of a *different domain* Dj (not just a different object). This is distinct from `owner`, which governs rights on an object, not on another domain.
- **`switch`/`transfer`** (as above) — permits a domain switch from Di to Dj.

> **Exam note (slide-specific framing):** the access-matrix design is explicitly presented as **separating mechanism from policy** — the OS's mechanism is providing the matrix + rules and ensuring only authorized agents can manipulate it; the policy is the user deciding who gets access to what. Note also that even with all of this, the access matrix does **not solve the general confinement problem** (preventing a program from leaking information it has legitimate access to, to a party that shouldn't have it) — worth stating as a known limitation if asked.

> **Exam note:** The access matrix is a **conceptual model**, not how it's literally stored — it's typically huge and sparse (most entries empty), so real systems implement it via the decompositions below rather than as a literal 2-D array.

### Implementation of the Access Matrix

| Implementation | Structure | Trade-off |
|---|---|---|
| **Global table** | One flat table of `⟨domain, object, rights⟩` triples | Simple, but the table itself can be very large; hard to group entries meaningfully |
| **Access list per object (ACL)** | Each object stores its own column: a list of `⟨domain, rights⟩` pairs | Natural fit for "who can access this file" queries. Default rights for domains not listed can be handled with a wildcard/default entry |
| **Capability list per domain** | Each domain stores its own row: a list of `⟨object, rights⟩` pairs — each entry called a **capability** | Natural fit for "what can this process access" — this is the object-holder's ticket to the resource. The list itself must be protected from tampering by the process (kept in protected/kernel-controlled memory) |
| **Lock-key mechanism** | Each object has a list of unique bit-pattern **locks**; each domain has a list of **keys**. A domain may access an object only if it holds a key matching one of the object's locks | Compromise between ACLs and capability lists — a domain's key list can be changed without touching every object, and an object's lock list can be changed without touching every domain |

> **Key skill:** Given a scenario, identify whether "who can access this object" (→ access list/ACL) or "what can this process access" (→ capability list) is being asked, and name the matching implementation.

---

## Topic 30 — Access Rights: Revocation, MAC, Capability-Based Systems
**Ref: Silberschatz 17.7, 17.9, 17.10, 17.11**

### Revocation of Access Rights
In a dynamic protection system, rights granted earlier may need to be taken back. Revocation questions to consider:

| Dimension | Options |
|---|---|
| **Immediate vs. Delayed** | Does revocation take effect right away, or only at some later point (e.g., next access attempt)? |
| **Selective vs. General** | Does it revoke rights for one specific domain, or for all domains holding that right? |
| **Partial vs. Total** | Does it revoke a subset of the rights held on an object, or all of them? |
| **Temporary vs. Permanent** | Can the revoked rights be re-granted later, or is the revocation final? |

**Revocation under access lists:** straightforward — search the object's access list and delete or modify the relevant entry. Effect is immediate, and can be selective, general, partial, or total as needed.

**Revocation under capability lists:** harder, precisely *because* capabilities are distributed copies scattered across domains rather than centralized at the object — there's no single list to edit. Common schemes:

| Scheme | Mechanism |
|---|---|
| **Reacquisition** | Capabilities are periodically deleted from each domain; a domain must re-request the capability, at which point revocation is naturally enforced if no longer authorized |
| **Back-pointers** | Object keeps a pointer to every capability issued for it, so all copies can be located and deleted directly. Powerful but expensive to maintain |
| **Indirection (via a global table)** | Capabilities point to an entry in a global table, not directly to the object. Revocation deletes the global-table entry — every capability referencing it is invalidated at once, without needing to find each copy |
| **Keys** | Each capability carries a unique key value, matched against a key stored with the object. Revocation = change the object's key, invalidating all outstanding capabilities holding the old key. A **master key** can regenerate new keys and selectively re-issue capabilities |

> **Exam note:** The core tension to state clearly: access lists make revocation trivial (edit one list) precisely because rights are centralized at the object; capabilities make revocation hard precisely because rights are decentralized at the domain — indirection/keys exist specifically to recover a "central point of control" for capability-based systems.

### Capability-Based Systems
A system built around capabilities as the fundamental protection primitive (rather than layering capabilities on top of a conventional file-system-style ACL model).

- A **capability** is an unforgeable token that names an object and lists the operations permitted on it. Possession of the capability *is* the proof of authorization — no separate lookup against an object-side list is needed.
- Unforgeability must be enforced by the system, since a process holding a forged/tampered capability would gain unauthorized access. Typical enforcement: capabilities are tagged and only kernel-mode code may create or modify them; or they're protected using cryptographic techniques.
- **Historical origin:** **Hydra** (tag-based capabilities, with rights amplification allowing a capability to grant additional rights within a specific procedure's execution) and **CAP System** (data capabilities for hardware-recognized primitive rights + software capabilities interpreted by a protected supervisor procedure for more complex/user-defined rights) were the first capability-based systems.

**Modern capability systems — POSIX capabilities (slide-emphasized, Linux/Android):**
- Based on POSIX.1e (drafted, never became a formal standard), now used in Linux, Android, and others.
- Core idea: **slice up root's all-or-nothing power into distinct capability bits** — a bitmap, one bit per privileged operation area — instead of a process either having full root privilege or none.
- Fine-grained control is achieved by setting or masking individual bitmap bits.
- **Three bitmap sets**, applicable per-process or per-thread: **permitted**, **effective**, and **inheritable**.
- Once a capability bit is revoked, it **cannot be reacquired** — a process/thread typically starts with a fuller set of privileges and voluntarily *decreases* its own set as it runs, never the reverse.
- This is a direct, practical implementation of the **Principle of Least Privilege**: rather than "root or nothing," a process keeps only the specific privileged operations it actually still needs, dropping the rest as soon as they're no longer required.
- Improvement over the traditional root model, where adding one new fine-grained privilege category is inflexible/difficult.

### Other Protection Improvement Methods (slide-only — modern OS examples)
Not in Silberschatz's classical treatment, but explicitly covered in the slide deck:

| Method | Mechanism |
|---|---|
| **System Integrity Protection (SIP)** | Introduced by Apple, macOS 10.11. Restricts access to system files/resources **even from root**. Uses extended file attributes to mark a binary as restricted from modification/debugging; only code-signed kernel extensions and (configurably) code-signed apps are allowed. |
| **System-call filtering** | Acts like a firewall, but for system calls — can inspect calls and even their arguments. Linux implements this via **SECCOMP-BPF** (Berkeley Packet Filtering applied to syscalls). |
| **Sandboxing** | Runs a process in a restricted environment with an irremovable set of restrictions imposed *before* `main()` even starts. Java/.NET implement this at the virtual-machine level; other systems build it on MAC. Apple's early version was macOS 10.5's "seatbelt" (dynamic profiles written in Scheme, filtering syscalls down to the argument level) — now superseded by SIP as a system-wide profile. |

### Language-Based Protection
Protection specifications can be written directly into a programming language, describing high-level policies for resource allocation/use. The language implementation then either enforces this itself in software (where hardware-level checking isn't available) or translates the specification into calls on whatever protection mechanism the hardware/OS actually provides.

### Mandatory Access Control (MAC) vs. Discretionary Access Control (DAC)

| Model | Who decides access | Typical mechanism | Example use case |
|---|---|---|---|
| **DAC (Discretionary Access Control)** | The **owner** of an object decides who else may access it, and can change this at will | Access matrix / ACLs — the standard model discussed above | General-purpose OS file permissions (e.g., UNIX `rwx`, Windows ACLs) |
| **MAC (Mandatory Access Control)** | The **system** enforces access based on fixed security labels/classifications assigned to both subjects (processes) and objects — **not overridable even by root/the owner** | A subject's label is checked against an object's label before any access is granted | High-assurance systems: **Trusted Solaris**, **TrustedBSD** (used in macOS), **SELinux**, Windows Vista MAC |

> **Exam note:** The defining distinction is *who has the authority to change access* — under DAC it's the owner (discretionary, i.e. "at their discretion"); under MAC it's a system-enforced policy the owner **cannot** override (mandatory) — a slide-emphasized detail: DAC's core weakness is that it's discretionary, meaning users/admins must actively *choose* to increase protection; MAC removes that choice. Modern systems (Trusted Solaris, TrustedBSD/macOS, SELinux, Windows Vista) typically run **both** — DAC as the default, MAC as a stronger optional layer on top.

### Role-Based Access Control (RBAC)
A related, commonly-mentioned model: rights are assigned to **roles** (e.g., "backup operator," "administrator") rather than directly to individual users; users are then assigned to roles. Simplifies administration in large systems — changing a role's rights automatically updates every user in that role, and a user's access changes simply by reassigning their role membership.

---

## Final PYQ Priority (see [CSE321_final_probable_questions](../pyq/CSE321_final_probable_questions.md))
Based on 8 sampled Final sittings: **MEDIUM** overall — present in about half the current-era sittings (#8 on the ranked list), always small (2–5 marks), and always a single applied scenario: an access-matrix right in use (Owner/Copy, "draw the updated matrix"), or a "domain is static, how do you add rights" scenario. The deeper taxonomy in this note — revocation schemes, POSIX capability bitmaps, SIP/sandboxing, MAC vs DAC — has **not** appeared as exam content in any sitting sampled; keep it for conceptual completeness, but weight actual revision time toward File Systems/Memory Management first, then the access-matrix mechanics specifically within this module.

## Module 7 — Quick Review

| # | Topic | Must Know |
|---|---|---|
| 29 | Protection goals & access matrix | Mechanism vs Policy; Protection vs Security distinction; domain of protection + need-to-know + domain switching; **UNIX domain implementation (setuid/su/sudo)** and Android (per-app UID/GID); access matrix structure; all 4 special rights (`copy`, `owner`, `control`, `switch`/`transfer`); 4 implementations (global table, ACL, capability list, lock-key) |
| 30 | Revocation, MAC, capabilities | 4 revocation dimensions; why ACL revocation is easy and capability revocation is hard; 4 capability-revocation schemes; capability-based systems — historical (Hydra, CAP) **and** modern (POSIX capabilities: permitted/effective/inheritable bitmaps); SIP, system-call filtering, sandboxing; language-based protection; MAC vs DAC with concrete examples (SELinux, TrustedBSD, Trusted Solaris) |

### Quick Reference — Access Matrix Implementations
```
"Who can access this object?"   → Access List (ACL) — stored per object
"What can this process access?" → Capability List — stored per domain
Compromise (both changeable independently) → Lock-Key mechanism
```

### Quick Reference — MAC vs DAC
```
DAC: owner decides, owner can change    → standard file permissions
MAC: system enforces, owner cannot override → classified/high-assurance systems
```
