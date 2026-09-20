---
source: CSE321_theory_module07_protection.pptx (the original, included in this repo)
fidelity: the summary above the divider was rewritten from the deck on 2026-09-20 after an audit found errors in the earlier version; it is still a condensed paraphrase and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 8 embedded image(s); informational images are described in the summary)
source-mtime: 2025-09-03T00:02:22
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Module 07: Protection

Lecture slides (19) on operating-system protection: goals, the principles of least privilege and mechanism vs policy, protection domains (structure, UNIX and Android implementations), the access matrix and its variants (domain switching, copy, owner and control rights), revocation of access rights, mandatory access control, capability-based systems (POSIX.1e), other improvement methods (SIP, system-call filtering, sandboxing) and language-based protection.
Pptx last modified: 03-09-2025.
Rewritten 2026-09-20 directly from the deck (an audit found the earlier summary unreliable). Access-matrix tables and figures below were read from renders of slides 5, 8, 10–13 and 17.

## Content

### Slides 2–4: Goals, principles, domains
- **Slide 2, goals:** an OS is a collection of objects (hardware or software); each object has a unique name and is accessible through a defined set of operations; set policies to ensure authorized access to objects.
- **Slide 3:** *principle of least privilege* — programs, users and systems get just enough privileges to perform their tasks, so a component's failure or compromise causes the least damage. *Mechanism vs policy* — mechanism is what the OS builds in for protection; policy defines which states are allowed (authorized) or not for a given system.
- **Slide 4, domain of protection:** rings of protection separate functions into domains and order them hierarchically. A computer can be treated as processes and objects (hardware objects such as devices; software objects such as files, programs, semaphores). A process should access only the objects it currently needs — the *need-to-know* principle. Implemented by a process running in a *protection domain*, which specifies the resources it may access; each domain specifies a set of objects and the operations allowed on them; the ability to execute an operation on an object is an *access right* `<object-name, rights-set>`; domains may share access rights; associations can be static or dynamic (if dynamic, a process can domain-switch).
- **Slide 5, domain structure:** domain = a set of access rights; a rights-set is a subset of the valid operations on the object. Figure — D1 = {⟨O3, {read, write}⟩, ⟨O1, {read, write}⟩, ⟨O2, {execute}⟩}; D2 = {⟨O2, {write}⟩, ⟨O4, {print}⟩}; D3 = {⟨O4, {print}⟩, ⟨O1, {execute}⟩, ⟨O3, {read}⟩}; D2 and D3 overlap and share ⟨O4, {print}⟩.

### Slides 6–7: Domain implementations
- **Slide 6, UNIX:** domain = user-id. Domain switch via the file system: each file has a domain bit (the *setuid* bit); executing a file with setuid on sets the user-id to the file's owner until execution completes, then it is reset. Via passwords: `su` temporarily switches to another user's domain when that domain's password is given. Via commands: the `sudo` prefix runs a command in another domain if the original domain has privilege or the password is given.
- **Slide 7, Android app IDs:** each application gets a distinct user ID (UID) and group ID (GID) from the `installd` daemon when installed, plus a private data directory `/data/data/<appname>` owned by that UID/GID alone — the same protection UNIX gives separate users; quick, simple isolation, security and privacy. The kernel is modified to allow certain operations (such as networking sockets) only to members of a particular GID (e.g. `AID_INET`, 3003); Android can also mark certain UIDs "isolated", preventing them from initiating RPC requests to more than a bare minimum of services.

### Slides 8–13: The access matrix
- **Slide 8:** protection viewed as a matrix. Columns = access-control list (ACL) for an object (represent objects); rows = capability list (permitted operations per domain) (represent domains); access(i, j) is the set of operations a process in domain Dᵢ can invoke on object Oⱼ. Matrix: objects F1, F2, F3, printer; D1 — F1 read, F3 read; D2 — printer print; D3 — F2 read, F3 execute; D4 — F1 read/write, F3 read/write.
- **Slide 9, use:** if a process in Dᵢ tries operation `op` on Oⱼ, `op` must be in the matrix; the creator of an object defines its column; dynamic protection adds operations to add/delete rights and special rights: *owner* of Oᵢ, *copy* `op` from Oᵢ to Oⱼ (denoted `*`), *control* (Dᵢ can modify Dⱼ's rights), *transfer* (switch from Dᵢ to Dⱼ); copy and owner apply to objects, control to domain objects. The matrix design separates mechanism (the OS provides the matrix and rules, ensures only authorized agents manipulate it and rules are enforced) from policy (the user dictates who can access what, and how). It does not solve the general confinement problem.
- **Slide 10, domains as objects (switch right):** a switch from Dᵢ to Dⱼ is possible only if `switch ∈ access(i, j)`. Matrix: columns F1, F2, F3, laser printer, D1, D2, D3, D4; D1 — F1 read, F3 read, D2 switch; D2 — laser printer print, D3 switch, D4 switch; D3 — F2 read, F3 execute; D4 — F1 read/write, F3 read/write, D1 switch.
- **Slide 11, copy rights (`*`):** an asterisk means the right can be copied within its column. Example: a process in D2 copies the read operation into another entry for file F2. Matrix (a) before: D1 — F1 execute, F3 write*; D2 — F1 execute, F2 read*, F3 execute; D3 — F1 execute. Matrix (b) after: D3 additionally gets F2 read.
- **Slide 12, owner rights:** ownership lets a domain add or remove rights. (a) D1 — F1 owner/execute, F3 write; D2 — F2 read*/owner, F3 read*/owner/write; D3 — F1 execute. (b) after D1 and D2 use owner rights: D2 — F2 owner/read*/write*, F3 read*/owner/write; D3 — F2 write, F3 write (D1 unchanged).
- **Slide 13, control rights:** a process in one domain can modify another domain — here a process in D2 could modify D4. Matrix: D1 — F1 read, F3 read, D2 switch; D2 — laser printer print, D3 switch, D4 switch/control; D3 — F2 read, F3 execute; D4 — F1 write, F3 write, D1 switch.
- **Slide 14, revocation of access rights.** *Access-list scheme:* search for the right to revoke and delete it; may be immediate or delayed, selective or general, partial or total, temporary or permanent. *Capabilities:* must identify capabilities before revoking; methods — reacquisition (try to reacquire after deletion), back-pointers (point from object to capabilities; expensive; used in MULTICS), indirection (capability points to a table entry; not selective), keys (one key per capability; checked against a global key table).

### Slides 15–19: Modern protection
- **Slide 15, mandatory access control (MAC):** traditional OSs use discretionary access control (DAC — UNIX file permissions, Windows ACLs), a weakness because users/admins must act to increase protection. MAC, which even root cannot circumvent, makes resources inaccessible except to their intended owners; modern systems implement both, MAC usually as an optional, more secure configuration (Trusted Solaris, TrustedBSD (used in macOS), SELinux, Windows Vista MAC). At its heart, labels are assigned to objects and subjects (including processes); when a subject requests access, policy checks whether a subject with that label may perform the action on that object.
- **Slides 16–17, capability-based systems:** Hydra and CAP were the first; now in Linux, Android and others, based on POSIX.1e (never a standard). Root powers are sliced into distinct areas, each a bit in a bitmap, giving fine-grained control over privileged operations; three bitmaps — permitted, effective, inheritable — per process or per thread; once revoked a capability cannot be reacquired; a process starts with all privileges and voluntarily decreases them — essentially a direct implementation of least privilege, an improvement over all-powerful root. **Slide 17 figure:** a pie of capabilities (`CAP_CHOWN`, `CAP_SETUID`, `CAP_KILL`, `CAP_NET_ADMIN`, `CAP_NET_RAW`, …) with the note that in the old model even `ping` needed root because it opens a raw ICMP socket, whereas with capabilities `ping` can run as a normal user with only `CAP_NET_RAW` set.
- **Slide 18, other methods:** *System Integrity Protection (SIP)* — introduced by Apple in macOS 10.11; restricts access to system files and resources even by root, using extended file attributes to mark binaries that restrict changes and disable debugging; only code-signed kernel extensions allowed. *System-call filtering* — like a firewall for system calls, can inspect arguments; Linux implements it with SECCOMP-BPF (Berkeley packet filtering). *Sandboxing* — run a process in a limited environment by imposing irremovable restrictions early in startup (before `main()`), so it cannot access resources beyond its allowed set; Java and .NET do it at the virtual-machine level, other systems use MAC; Apple was an early adopter (macOS 10.5 "seatbelt", dynamic profiles written in Scheme managing system calls down to the argument level) and now uses SIP as a system-wide platform profile.
- **Slide 19, language-based protection:** protection specified in a programming language allows high-level description of policies for resource allocation and use; the language implementation can supply software enforcement when hardware checking is unavailable and interprets protection specifications to generate calls on whatever protection system the hardware and OS provide.

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets; list numbering, formatting and images are not reproduced -->

<!-- slide 1 -->
### OPERATING SYSTEMS Protection

<!-- slide 2 -->
### Goals
Operating system consists of a collection of objects (hardware or software).
Each object has a unique name and are accessible through some defined set of operations.
Set policies to ensure authorized access of objects within the computer system.

<!-- slide 3 -->
### Principles of Protection
Principle of Least Privilege
Programs, users and systems should be given just enough privileges to perform their tasks.
A component's failure or compromise causes the least amount of damage.
Mechanism vs Policy
Mechanism is what is built into the OS for its protection.
Policy defines what states are allowed (i.e. authorized) or not allowed (i.e. unauthorized) for a given system.

<!-- slide 4 -->
### Domain of Protection
Rings of protection separate functions into domains and order them hierarchically.
Computer can be treated as processes and objects.
  Hardware objects (such as devices) and software objects (such as files, programs, semaphores.
Process for example should only have access to objects it currently requires to complete its task – the need-to-know principle.
Implementation can be via process operating in a protection domain.
  Specifies resources process may access.
  Each domain specifies set of objects and types of operations on them
  Ability to execute an operation on an object is an access right.
    <object-name, rights-set>
  Domains may share access rights.
  Associations can be static or dynamic.
  If dynamic, processes can domain switch.

<!-- slide 5 -->
### Domain Structure
Need-to-know-principle:
Processes should only be allowed to access resources which are necessary for completing tasks and for which they are authorized.
Domain => set of access-rights
Access-right = <object-name, rights-set> where rights-set is a subset of all valid operations that can be performed on the object.
[image]

<!-- slide 6 -->
### Domain Implementation in UNIX
Domain = user-id
Domain switch accomplished via file system.
  Each file has associated with it a domain bit (setuid bit).
  When file is executed and setuid = on, then user-id is set to owner of the file being executed.
  When execution completes user-id is reset.
Domain switch accomplished via passwords.
  su command temporarily switches to another user’s domain when other domain’s password provided.
Domain switching via commands.
  sudo command prefix executes specified command in another domain (if original domain has privilege or password given).

<!-- slide 7 -->
### Domain Implementation in Android App IDs
In Android, distinct user IDs are provided on a per-application basis.
When an application is installed, the installd daemon assigns it a distinct user ID (UID) and group ID (GID), along with a private data directory (/data/data/<appname>) whose ownership is granted to this UID/GID combination alone.
Applications on the device enjoy the same level of protection provided by UNIX systems to separate users.
A quick and simple way to provide isolation, security, and privacy.
The mechanism is extended by modifying the kernel to allow certain operations (such as networking sockets) only to members of a particular GID (for example, AID INET, 3003).
A further enhancement by Android is to define certain UIDs as “isolated,” prevents them from initiating RPC requests to any but a bare minimum of services.

<!-- slide 8 -->
### Access Matrix
View protection as a matrix (access matrix).
Columns => access-control list (ACL) for an object => represents objects.
Rows => capability list (permissible operations on objects, per domain) => represents domains.
Access(i, j) is the set of operations that a process executing in Domaini can invoke on Objectj
[image]

<!-- slide 9 -->
### Use of Access Matrix
If a process in Domain Dᵢ tries to do “op” on object Oⱼ, then “op” must be in the access matrix.
User who creates object can define access column for that object.
Can be expanded to dynamic protection.
  Operations to add, delete access rights.
  Special access rights:
    owner of Oᵢ
    copy “op” from Oᵢ to Oⱼ (denoted by “*”)
    control – Dᵢ can modify Dⱼ access rights
    transfer – switch from domain Dᵢ to Dⱼ
  Copy and Owner applicable to an object.
  Control applicable to domain object.
Access matrix design separates mechanism from policy.
  Mechanism:
    Operating system provides access-matrix + rules.
    If ensures that the matrix is only manipulated by authorized agents and that rules are strictly enforced.
  Policy:
    User dictates policy.
    Who can access what object and in what mode.
But doesn’t solve the general confinement problem.

<!-- slide 10 -->
### Access Matrix with Domains as Object
Switching Domains
Domain switch from Di to Dj is possible only if access right switch ∈ access(i, j)
[image]

<!-- slide 11 -->
### Access Matrix with Copyrights
Copyrights
Asterisk denotes that an access right can be copied within column.
[image]
Process executing in domain D2 copies the read operation into another entry associated with file F2

<!-- slide 12 -->
### Access Matrix with Owner Rights
Owner Rights
Ownership: Can add or remove rights.
[image]
[image]

<!-- slide 13 -->
### Access Matrix with Control Rights
Control Rights
Control: Process executing in one domain can modify another domain.
A process executing in domain D2 could modify domain D4
[image]

<!-- slide 14 -->
### Revocation of Access Rights
Access-List Scheme
Search for right to be revoked, then delete.
Immediate or Delayed.
Selective or General.
Partial or Total.
Temporary or Permanent.
Capabilities
Identify capabilities before revoking them
| Reacquisition / Try to reacquire after deletion | Back-pointers: point from object to capabilities / Expensive (used in MULTICS) |
| Indirection / Capability points to entry in table / Not selective | Keys / One key per capability / Check in global key table |

<!-- slide 15 -->
### Mandatory Access Control (MAC)
Operating systems traditionally had discretionary access control (DAC) to limit access to files and other objects (for example UNIX file permissions and Windows access control lists (ACLs)).
  Discretionary is a weakness – users / admins need to do something to increase protection.
Stronger form is mandatory access control, which even root user can’t circumvent.
  Makes resources inaccessible except to their intended owners.
  Modern systems implement both MAC and DAC, with MAC usually a more secure, optional configuration (Trusted Solaris, TrustedBSD (used in macOS), SELinux), Windows Vista MAC).
At its heart, labels assigned to objects and subjects (including processes).
  When a subject requests access to an object, policy checked to determine whether or not a given label-holding subject is allowed to perform the action on the object.

<!-- slide 16 -->
### Capability-Based Systems
Hydra and CAP were first capability-based systems.
Now included in Linux, Android and others, based on POSIX.1e (that never became a standard).
  Essentially slices up root powers into distinct areas, each represented by a bitmap bit.
  Fine grain control over privileged operations can be achieved by setting or masking the bitmap.
  Three sets of bitmaps – permitted, effective, and inheritable.
    Can apply per process or per thread.
    Once revoked, cannot be reacquired.
    Process or thread starts with all privs, voluntarily decreases set during execution.
    Essentially a direct implementation of the principle of least privilege.
An improvement over root having all privileges but inflexible (adding new privilege difficult, etc.).

<!-- slide 17 -->
### Capabilities in POSIX.1e
[image]

<!-- slide 18 -->
### Other Protection Improvement Methods
System integrity protection (SIP):
  Introduced by Apple in macOS 10.11.
  Restricts access to system files and resources, even by root.
  Uses extended file attributes to mark a binary to restrict changes, disable debugging and scrutinizing.
  Also, only code-signed kernel extensions allowed and configurable only code-signed apps.
System-call filtering:
  Like a firewall, for system calls.
  Can also be deeper –inspecting all system call arguments.
  Linux implements via SECCOMP-BPF (Berkeley packet filtering).
Sandboxing:
  Running process in limited environment.
  Impose set of irremovable restrictions early in startup of process (before main()).
  Process then unable to access any resources beyond its allowed set.
  Java and .net implement at a virtual machine level.
  Other systems use MAC to implement.
  Apple was an early adopter, from macOS 10.5’s “seatbelt” feature.
    Dynamic profiles written in the Scheme language, managing system calls even at the argument level.
    Apple now does SIP, a system-wide platform profile.

<!-- slide 19 -->
### Language-Based Protection
Specification of protection in a programming language allows the high-level description of policies for the allocation and use of resources.
Language implementation can provide software for protection enforcement when automatic hardware-supported checking is unavailable.
Interpret protection specifications to generate calls on whatever protection system is provided by the hardware and the operating system.
