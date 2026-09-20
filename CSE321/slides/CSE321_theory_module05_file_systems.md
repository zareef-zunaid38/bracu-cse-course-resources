# CSE321 Module 05: File Systems & Secondary Storage

Lecture slides covering secondary storage structures, hard disk drive anatomy, disk scheduling algorithms (FCFS, SSTF, SCAN, C-SCAN, LOOK, C-LOOK), file concepts, access methods, directory structures, and file system mounting/protection.
Pptx last modified: 10-08-2025.

## Content

### Slides 1–8: Magnetic Disk Structure & Overview
- Magnetic disks provide bulk of secondary storage for modern systems (non-volatile permanent storage).
- **Structure Anatomy:**
  - Multiple circular platters coated with magnetic material.
  - Each platter has two surfaces (upper and lower) accessed by read-write heads.
  - Platters mounted on a central **spindle** rotating at 5,400 to 15,000 RPM.
  - Surface divided logically into circular **tracks**, subdivided into **sectors** (typically 512 bytes or 4 KB).
  - Set of tracks at one arm position across all platters forms a **cylinder**.
  - Read-write heads attached to a disk arm assembly moving across tracks as a unit.
- **Disk Performance Metrics:**
  - **Seek Time:** Time for disk arm to move heads to the cylinder containing the desired sector.
  - **Rotational Latency:** Additional time waiting for disk to rotate target sector under read-write head.
  - **Transfer Rate:** Rate at which data flows between drive and computer.

### Slides 9–14: Disk Attachment & I/O
- Host communicates with disk drive through I/O bus (SATA, SAS, USB, Fibre Channel).
- Host-attached storage accessed via system I/O buses.
- Network-Attached Storage (NAS): Storage made available over a network rather than direct bus.
- Storage Area Network (SAN): Dedicated high-speed network connecting servers and storage units.

### Slides 15–33: Disk Scheduling Algorithms
OS responsible for using hardware efficiently; goal is to minimize average seek time:
1. **FCFS (First-Come, First-Served):**
   - Requests serviced in arrival order.
   - Fair, no starvation, but high total head movement.
2. **SSTF (Shortest Seek Time First):**
   - Selects request with minimum seek time from current head position.
   - Substantially reduces head movement compared to FCFS.
   - *Problem:* May cause starvation for distant requests if closer requests keep arriving.
3. **SCAN (Elevator Algorithm):**
   - Disk arm starts at one end of the disk and moves toward the other end, servicing requests until it reaches the other end, then reverses direction.
4. **C-SCAN (Circular SCAN):**
   - Designed to provide more uniform wait time. Arm moves in one direction servicing requests, then immediately returns to the beginning of the disk without servicing requests on the return trip.
5. **LOOK and C-LOOK:**
   - Variants of SCAN and C-SCAN where arm only goes as far as the final request in each direction before reversing or returning, rather than traveling to the full physical end of the disk.

### Slides 34–45: File Concept & Attributes
- **File:** Contiguous logical address space abstracting physical storage.
- **File Attributes:**
  - Name (human-readable), Identifier (unique tag/inode), Type, Location (pointer to device/blocks), Size, Protection (access permissions), Time/Date/User identification.
- **File Operations:**
  - Create, Write, Read, Reposition within file (`seek`), Delete, Truncate.
- **Open-File Table:** OS keeps table containing info about all open files (file pointer, file-open count, disk location, access rights).

### Slides 46–52: Access Methods
1. **Sequential Access:** Information processed in order, one record after another (read next, write next, reset). Most common (compilers, editors).
2. **Direct (Relative) Access:** Fixed-length logical records read/written in any order (`read n`, `write n`, `position to n`). Essential for database systems.
3. **Indexed Access:** Index contains pointers to various blocks for rapid lookup.

### Slides 53–62: Directory Structures
- Directory maps file names to directory entries.
- **Schemes:**
  - **Single-Level Directory:** All files in same directory. Unique naming problem, grouping impossible.
  - **Two-Level Directory:** Separate directory for each user. Resolves naming collision between users.
  - **Tree-Structured Directory:** Arbitrary depth directory tree with root. Absolute and relative pathnames.
  - **Acyclic-Graph Directory:** Allows directories to share subdirectories and files (links/aliasing). Danger of dangling pointers and cycle detection during traversal.
  - **General Graph Directory:** Allows arbitrary cycles; requires garbage collection reference counting.

### Slides 63–69: File System Mounting & Protection
- **Mounting:** File system must be mounted before it can be accessed. Mounted at a mount point in directory hierarchy.
- **Protection & Access Control:**
  - Control access by limiting types of file access: Read, Write, Execute, Append, Delete, List.
  - **UNIX Protection Classes:**
    - **Owner (User - u)**
    - **Group (g)**
    - **Universe / Others (o)**
  - 3 bits per class: `rwx` (read, write, execute), e.g., `chmod 761 file` (`rwxrw---x`).
