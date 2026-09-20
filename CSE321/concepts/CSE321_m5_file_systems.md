---
type: module-notes
course: CSE321
module: 5
topics: "20–23"
title: File Systems
status: draft
date: 2026-07-31
exam: final
---

# CSE321 — Module 5: File Systems
**Topics 20–23 | Exam: Final | Silberschatz Ch.11, 13, 14 | OSTEP Ch.40, 42**

---

## Topic 20 — File Concept: Attributes, Operations, Types
**Ref: Silberschatz 11.1.1**

### Magnetic Disk Structure (slide-led — the deck opens here)
The physical medium underneath the file concept, since the slide deck starts with hardware before software:

| Part | Role |
|---|---|
| **Platter** | Circular disk (like a CD); a disk stacks multiple platters on one spindle |
| **Spindle** | Rotates all platters together, unidirectionally |
| **Surface** | Each platter has an upper and lower surface, both used, each with its own read-write head |
| **Read-write head** | Fetches/writes data; moves back and forth across the surface — forward toward inner tracks, backward toward outer tracks |
| **Track** | Concentric ring on a surface. Outermost = **external track**, innermost = **internal track** |
| **Sector** | Each track is divided into a fixed number of sectors — the actual unit data is stored in |

> **Exam note (slide-specific fact):** moving the head toward an *inner* track = moving forward from the current position; toward an *outer* track = moving backward. This directional framing shows up in seek-related questions.

### File
A **file** is a named collection of related information recorded on secondary storage. It is the smallest unit of logical storage the OS exposes to a user/application — the OS maps the abstraction onto raw disk blocks.

### File Attributes

| Attribute | Meaning |
|---|---|
| **Name** | Human-readable identifier, kept in human-readable form |
| **Identifier** | Unique tag (usually a number) identifying the file within the file system — this is what the OS actually uses internally (e.g. the inode number in UNIX) |
| **Type** | Needed for systems that support multiple file types |
| **Location** | Pointer to the device and location of the file on that device |
| **Size** | Current file size (bytes/blocks), possibly max allowed size |
| **Protection** | Access-control information: who can read/write/execute |
| **Time, date, user ID** | Data for creation, last modification, last use — useful for protection, security, and usage monitoring |

### File Operations

| Operation | Description |
|---|---|
| **Create** | Allocate space, make an entry in the directory |
| **Open** | Bring file metadata into memory (open-file table) before use |
| **Write** | Write at the current file pointer, advance pointer |
| **Read** | Read at the current file pointer, advance pointer |
| **Reposition (seek)** | Move the file pointer without I/O — enables random access |
| **Delete** | Remove directory entry, release space |
| **Truncate** | Erase contents, keep attributes (size → 0) |
| **Close** | Flush and remove from open-file table |

> **Exam note:** Open/close exist because looking up a file's location/attributes on every single read/write would be expensive — open caches that lookup once per file-use session.

> **Slide-verification note:** the theory slide deck's own operation list names only six — Creating, Reading, Writing, Deleting, Truncating, Repositioning — and doesn't call out Open/Close as separate named operations the way Silberschatz does. Both framings are consistent (open/close are implicit in "creating"/normal use in the slide's simplified list); know the fuller 8-operation Silberschatz list above as the primary answer, but don't be thrown if a question uses the slide's shorter 6-item phrasing.

### File Types
Two common approaches:
- **By name + extension** — `.c`, `.exe`, `.txt`, `.zip`. OS uses the extension to decide default handling (e.g., double-click behavior), but the extension is only a convention, not enforced structure.
- **By internal structure** — some OSes (rare today) enforce a minimal internal structure (e.g., record-based files). Modern general-purpose OSes (UNIX/Linux, Windows NTFS) treat files as an unstructured byte stream — structure is imposed by the application, not the OS.

---

## Topic 21 — File System Structure and Implementation
**Ref: Silberschatz 13.1.1, 13.1.2**

### Layered File System

```
┌─────────────────────────────┐
│   Application Programs      │
├─────────────────────────────┤
│   Logical File System       │  ← manages metadata (FCB/inode), directory structure, protection
├─────────────────────────────┤
│   File-Organization Module  │  ← translates logical block # → physical block #, manages free space
├─────────────────────────────┤
│   Basic File System         │  ← issues generic commands to device driver (read/write physical block)
├─────────────────────────────┤
│   I/O Control                │  ← device drivers + interrupt handlers, translates commands to hardware ops
├─────────────────────────────┤
│   Devices                   │
└─────────────────────────────┘
```

Each layer only knows about the layer directly below it — the classic OS layering benefit (modularity, one implementation of a layer can be swapped without touching the others).

> **Faculty-verified correction (2026-08-18, checked directly against NTR's Lecture 5 slides):** this 6-box abstract layer stack is Silberschatz textbook content — it does **not** appear in NTR's own slides at all. Her framing for "how does a file system work" is different: she poses two questions ("The Way to Think") — **Data structures** (what on-disk structures organize data/metadata?) and **Access methods** (how does a syscall like open()/read()/write() map onto those structures — which get read for a given call?) — then answers both concretely via the **Ext2/VSFS block-group layout** below, not this abstract stack. For Quiz 3 specifically, prioritize the Ext2 layout over this diagram; keep this stack in mind for the Final (still valid Silberschatz content, Ref 13.1.1–13.1.2).

### NTR's Actual Framing — Ext2/VSFS Block-Group Layout
**Source: NTR's own Lecture 5 slides, verified 2026-08-18.** This is the concrete structure she teaches instead of the abstract layer stack above — and it's the same shape as the inode-address-calculation skill below (the ungraded HW's "5 reserved blocks then inode table" setup is a miniature version of this).

```
Disk: [Boot Sector | Block Group 0 | Block Group 1 | ... | Block Group N-1]

Each block group: [Super Block | Block Group Descriptor | Block Bitmap | Inode Bitmap | Inode Table | Data Blocks]
```

**Her worked example (block group 0):**

| Block offset | Length | Contents |
|---|---|---|
| byte 0 | 512 B | boot record |
| byte 512 | 512 B | additional boot record data |
| byte 1024 | 1024 B | superblock |
| block 2 | 1 block | block group descriptor table |
| block 3 | 1 block | block bitmap |
| block 4 | 1 block | inode bitmap |
| block 5 | 214 blocks | inode table |
| block 219 | 7974 blocks | data blocks |

Block group 1 (and later groups) repeat the same layout, typically carrying backup copies of the superblock and block group descriptor table.

**Inode table sizing example she gives:** inode table spans blocks 3–7, inode size 256 B → a 4 KB block holds 16 inodes → with 5 blocks reserved for the table, the file system supports 80 inodes total (= max number of files).

### On-Disk Structures

| Structure | Purpose |
|---|---|
| **Boot control block** | Info needed to boot an OS from that volume (per-volume, only meaningful if OS is stored there) |
| **Volume control block (superblock in UNIX, MFT in NTFS)** | Volume-wide info: total # of blocks, free block count, free block pointers, free FCB count |
| **Directory structure** | Organizes files — file names + associated inode/identifier numbers |
| **Per-file FCB (File Control Block)** | Called an **inode** in UNIX. Holds most file attributes (permissions, dates, owner, size, and — critically — pointers to the data blocks) |

### In-Memory Structures
Kept in RAM to avoid repeated disk reads for metadata already in use:

| Structure | Purpose |
|---|---|
| **Mount table** | Info about each mounted volume |
| **Directory-structure cache** | Recently-accessed directory info |
| **System-wide open-file table** | FCB copy for every currently open file, plus a count of processes that have it open |
| **Per-process open-file table** | Pointer into the system-wide table + the process's current file pointer |

> **Key skill:** Trace what happens on `open()` — logical file system searches the directory structure, finds the FCB (inode), copies it into the system-wide open-file table (or increments its open count if already resident), then creates an entry in the per-process table pointing at it. This is why `open()` is comparatively expensive and `read()`/`write()` afterward are cheap.

### Directory Organization
A directory holds a list of **(entry name, inode number) pairs** — nothing more. Looking up a file by name means scanning this list for a matching entry name, then following its inode number to the actual FCB.
- Every directory has two built-in entries: **`.`** (dot — points to itself, the current directory) and **`..`** (dot-dot — points to the parent directory).
- In most UNIX file systems, the **root directory's inode number is 2** (not 0 or 1 — a specific, quotable fact).

### Access Paths — Full I/O Trace for `open()`/`read()`/`write()`
**Ref: OSTEP Ch.40 (slide-emphasized — the practice sheet's Problems 3–4 are exactly this trace).** This is the standard exam question format for this topic: given a file path, illustrate the full disk-access timeline.

**Reading a file — `open("/foo/bar", O_RDONLY)` then `read()`:**
```
open() path traversal:
1. Begin at the root of the file system (root inode number = 2, most UNIX systems).
2. Read the block containing the root inode.
3. Look inside it to find the pointer to the root's data blocks (its directory contents).
4. Read the root's directory data block(s) to find the entry for "foo".
5. Read the block containing foo's inode.
6. Look inside foo's inode to find its data blocks, read them to find the entry for "bar".
7. Read the block containing bar's inode.
8. Check permissions.
9. Allocate a file descriptor for this process, return it to the user.

read() (after open() has completed):
10. Consult bar's (now-cached) inode to find the location of the first data block.
11. Read that data block.
12. Update the inode's last-accessed time.
13. Update the in-memory open-file table entry (file offset) for this descriptor.

close():
14. Deallocate the file descriptor. No disk I/O required — this is purely an in-memory bookkeeping step.
```

**Writing to a file — `create()` then `write()`:**
```
A single write() that allocates a new block generates 5 disk I/Os:
1. Read the data bitmap  (find a free block)
2. Write the data bitmap (mark it used)
3. Read the inode        (to update it)
4. Write the inode       (record the new block's pointer, update size)
5. Write the actual data block itself
```
Creating the file in the first place (before the write) additionally requires allocating space for a new directory entry — which itself causes further I/O traffic (its own inode, bitmap, and data-block updates for the parent directory).

> **Key skill:** Given a file path and a scenario ("file X, allocated in N data blocks, opened then read" / "file Y created then written, ends up in N data blocks"), enumerate the full step-by-step I/O timeline like the two traces above — this is precisely what the practice sheet asks for (Problems 3 and 4), not just a one-line summary.

---

## Topic 22 — File Allocation: Indexed Allocation and the UNIX Inode
**Ref: Silberschatz 14.4.3 | OSTEP Ch.40**

### Indexed Allocation
Each file gets its own **index block** — an array of pointers to the file's data blocks. Solves external fragmentation (unlike contiguous allocation) and supports direct/random access (unlike linked allocation, which requires traversal).

Problem: a single index block has a fixed number of pointers, so it can only address a limited file size. The **UNIX inode** solves this with a *multi-level* indexed structure.

### UNIX Inode Structure (classic, as in OSTEP's VSFS / Silberschatz)

```
inode
 ├── metadata (type, size, owner, permissions, timestamps, link count)
 └── block pointers:
       12 × direct pointers        → straight to data blocks
       1  × single indirect ptr    → points to a block full of direct pointers
       1  × double indirect ptr    → points to a block full of single-indirect pointers
       1  × triple indirect ptr    → points to a block full of double-indirect pointers
```

```
                         inode
                    ┌───────────┐
Direct[0..11] ───► [data][data][data]...  (12 blocks)
                    └───────────┘
Single Indirect ──► [ptr][ptr][ptr]...──► [data][data]...
                                          (each ptr → 1 data block)
Double Indirect ──► [ptr][ptr]...──► [ptr][ptr]...──► [data]...
                                     (indirect blocks)  (data blocks)
```

### Key Skill (1) — Max File Size Calculation
Given block size, pointer size, and how many of each pointer type the inode has, compute how many pointers fit per block, then sum addressable size across all levels.

**Worked example — actual practice-sheet Problem 1:** 8 direct blocks, 2 single indirect, 2 double indirect, 2 triple indirect. Block size = 32 bytes, block-address (pointer) size = 4 bytes.
```
Pointers per block = 32 / 4 = 8

Direct:            8 blocks              × 32 B = 256 B
Single indirect:    2 × 8                × 32 B = 512 B
Double indirect:    2 × 8²  = 2 × 64     × 32 B = 4,096 B
Triple indirect:    2 × 8³  = 2 × 512    × 32 B = 32,768 B

Max file size = 256 + 512 + 4,096 + 32,768 = 37,632 bytes
```
> Note the structure: this inode has **2 of each** indirect pointer type (not the usual "1 single, 1 double, 1 triple" textbook default) — always read the problem's exact pointer counts, don't assume the classic 12-1-1-1 layout.

**Generic version (classic 12-1-1-1 layout)** — block size = 4 KB, pointer size = 4 bytes → pointers per block = 4096 / 4 = **1024**:

| Level | Blocks addressed | Size addressed |
|---|---|---|
| 12 direct | 12 | 12 × 4 KB = 48 KB |
| 1 single indirect | 1024 | 1024 × 4 KB = 4 MB |
| 1 double indirect | 1024 × 1024 = 1,048,576 | ≈ 4 GB |
| 1 triple indirect | 1024³ | ≈ 4 TB |

**Disk accesses to read a given logical block** (assuming the inode itself is already cached in memory, which is the standard exam assumption unless stated otherwise):

| Block location  | Accesses | Why                                                                                 |
| --------------- | -------- | ----------------------------------------------------------------------------------- |
| Direct block    | 1        | Read the data block directly                                                        |
| Single indirect | 2        | Read the indirect block (to get the pointer), then the data block                   |
| Double indirect | 3        | Read the double-indirect block, then the single-indirect block, then the data block |
| Triple indirect | 4        | One extra level of indirection                                                      |

> **If the inode is NOT assumed cached**, add 1 access at every level (read inode first). State the assumption explicitly in an exam answer.

### Key Skill (2) — Inode Byte/Sector Address Calculation
A **different** skill from the disk-access count above: given an inode *number*, find its physical location on disk. This is the actual practice-sheet Problem 2 style of question.

**Step 1 — byte offset into the inode table:**
```
offset = inode_number × size_of_inode
byte_address = inode_table_start_address + offset
```
**Worked example (slide's own numbers)** — inode number 32, inode size 256 bytes, inode table starts at 12 KB:
```
offset = 32 × 256 = 8,192 B = 8 KB
byte address = 12 KB + 8 KB = 20 KB
```

**Step 2 — if the disk is sector-addressable, not byte-addressable** (the practical case — disks address in fixed sectors, e.g. 512 B or 256 B, not individual bytes):
```
block# = (inode_number × size_of_inode) / block_size
sector_address = (block# × block_size + inode_table_start_address) / sector_size
```
**Worked example** — inode number 32, inode size 256 B, block size 4 KB, inode table start 12 KB:
```
block# = (32 × 256) / 4096 = 8192 / 4096 = 2
sector_address = (2 × 4096 + 12288) / 512 = (8192 + 12288) / 512 = 20480 / 512 = 40
```

> **Exam note:** Practice Problem 2 asks exactly this — given inode size, block size, sector size, and which blocks are reserved for superblock/bitmaps before the inode table, compute the sector address of a specific inode number. Don't confuse this with Key Skill (1)/the disk-access-count table above — that answers "how many reads to get file data," this answers "where on disk does inode N physically live."

### Ungraded HW (NTR, shared ~2026-08-16, no submission) — same skill, new numbers
Inode size = 256 B, block size = 16 KB. First 5 blocks: superblock, journal, group descriptor, data bitmap, inode bitmap. Next 5 blocks: inode table. Disk is sector-addressable, sector size = 512 B. Find the **byte address** and **sector address** of inode number 200.
- Not yet solved — in progress as of this session (2026-08-18), working from the same method NTR's own Lecture 5 slides use (verified identical to the worked example above: inode #32/256B/12KB start → offset 8192B → 20KB byte address → sector 40).
- **Method to apply:** inode table start = 5 × 16 KB = 80 KB; offset = 200 × 256 B; byte address = 80 KB + offset; block# = offset ÷ 16 KB (relative to table start); sector address = (block# × 16 KB + 80 KB) ÷ 512 B.

---

## Topic 23 — Crash Consistency and Journaling
**Ref: OSTEP Ch.42**

### The Crash Consistency Problem
A single logical file-system operation (e.g., appending a block to a file) typically requires **multiple physical disk writes**: update the data bitmap, update the inode's pointers/size, write the new data block. These writes are not atomic — a crash (power loss, OS panic) between them can leave the file system in an inconsistent state (e.g., a block marked "in use" in the bitmap but not referenced by any inode, or referenced by an inode but not marked used).

### Older Approach — fsck (File System Checker)
Runs at reboot, scans the **entire disk**, cross-checks bitmaps against inode pointers and directory structure, and repairs inconsistencies it can detect.
- **Problems:** slow (must scan the whole disk, which scales badly with disk size), and it can only catch structural inconsistencies — it does not guarantee recovery of the *specific* operation that was in progress, only that the file system is left structurally sane.

### Journaling (Write-Ahead Logging)
Before applying an update in-place, first write a description of the update to a **journal (log)** on disk. Only after the journal write is safely on disk does the file system apply ("checkpoint") the actual update to its final location.

**Basic journaling protocol (per transaction):**

```
1. Journal write:   TxB (transaction begin) + all pending metadata/data blocks + TxE (transaction end)
                     — written to the log, NOT the final location
2. Checkpoint:       once the journal entry (including TxE) is safely on disk,
                     write the actual blocks to their final in-place locations
```

- If a crash happens **before** TxE is written: the transaction is incomplete — discard it on recovery, nothing was checkpointed yet, disk is still in its old consistent state.
- If a crash happens **after** TxE but **before** checkpointing completes: on reboot, **redo** the transaction by replaying the journal — this recovers the operation instead of losing it.

### Journaling Modes
| Mode | What's journaled | Notes |
|---|---|---|
| **Physical/data journaling** | Both metadata and data blocks | Safest, but doubles the I/O (write once to log, once to final location) |
| **Metadata-only (ordered) journaling** | Only metadata; data blocks written directly to final location *before* the metadata journal entry | Common default (e.g., ext3/ext4 ordered mode) — good balance of safety and performance |

> **Exam note:** Recovery after a crash with journaling only requires scanning the (small) log, not the whole disk — this is the core advantage over fsck.

### Journal Commit — the Precise Write Order
To avoid the journal itself being ambiguous about whether a transaction is complete, writes to the log happen in a specific order:
```
1. Write all blocks EXCEPT TxE to the journal (TxB with id, then metadata/data blocks).
2. Only once that completes, issue the write of TxE.
```
This two-step split matters because TxE's presence is exactly what recovery uses to decide "was this transaction fully logged?" Writing everything-but-TxE first, then TxE last, means a crash can never leave a partially-written transaction looking complete.

**Full protocol, named steps:** Journal write → **Journal commit** (write the TxE/commit block) → Checkpoint (write contents to final on-disk locations) → **Free** (mark the transaction free in the journal superblock, once checkpointed).

### Making the Log Finite (Circular Log)
The journal itself is a fixed-size region, reused over and over — not infinite. A **journal superblock** tracks which transactions are the oldest and newest still-uncommitted-to-disk (not yet checkpointed), so the log can safely wrap around and reclaim space from transactions that are already checkpointed.

### Batching Log Updates
If two files are created in the same directory in quick succession, naively journaling each one separately would write and commit the same directory-entry block to the log twice. To reduce this write traffic, the file system **batches** pending updates into a single global transaction — written either when forced by a synchronous request, or after a timeout (e.g., every 5 seconds).

### Tricky Case — Block Reuse (Revoke Records)
A subtle recovery bug: suppose directory "foo" is deleted (freeing block 1000), then a new file "foobar" is created and happens to reuse block 1000 for its data. If a crash occurs while both transactions are still in the log, naive replay-everything-in-order recovery would **replay the old "foo" directory write to block 1000**, overwriting foobar's actual new data with stale directory contents.
- **Solution 1:** don't allow the freed block to be reused until it's actually been checkpointed.
- **Solution 2 (the general fix):** when deleting a directory, record a **revoke** entry in the journal — recovery checks for revoke records and skips replaying any logged write to a block that was later revoked.

---

## Final PYQ Priority (see [CSE321_final_probable_questions](../pyq/CSE321_final_probable_questions.md))
Based on 8 sampled Final sittings, weighted toward the current Sp25–Sp26 era:
- **HIGH:** Inode byte/sector address calc (near-certain, #2 on the ranked list) · Inode max-file-size calc, incl. inverted "solve for unknown pointer count" variants (#4) · Crash-consistency deduction from a partial-write scenario (#5) · File access path I/O timeline (#6)
- **MEDIUM:** File concept/attributes, directory organization — small conceptual sub-questions, not heavily marked on their own
- File Systems as a whole is now a fixture in every recent sitting (4/4), usually the largest or second-largest section — the highest-leverage module to drill for this exam alongside Memory Management.

## Module 5 — Quick Review

| # | Topic | Must Know |
|---|---|---|
| 20 | File concept | Disk hardware (platter/spindle/head/track/sector, inner=forward/outer=backward seek direction); attribute list; 6-op slide list vs 8-op Silberschatz list; extension vs internal-structure typing |
| 21 | FS structure | 5-layer stack; on-disk vs in-memory structures; inode = UNIX's FCB; directory = (name, inode#) pairs + `.`/`..`; root inode = 2; full open/read I/O trace; write = 5 I/Os per block appended |
| 22 | Indexed allocation / inode | 12 direct + single/double/triple indirect (or however many the problem states — don't assume 12-1-1-1); max-file-size calc; disk-access count per indirection level; **separately**, inode byte/sector address calc (offset = inode# × inode size) |
| 23 | Crash consistency | Why multi-write ops aren't atomic; fsck vs journaling; TxB→(metadata/data)→TxE write order; checkpoint; journal superblock (circular log); batching; block-reuse/revoke records |

### Quick Reference — Indirection Math
```
Pointers per block = block size / pointer size
Direct:            12 blocks  (or however many the problem states)
Single indirect:   (pointers per block) blocks
Double indirect:   (pointers per block)² blocks
Triple indirect:   (pointers per block)³ blocks

Disk accesses (inode cached): direct=1, single=2, double=3, triple=4
```

### Quick Reference — Inode Address Calculation
```
Byte:   offset = inode# × inode_size ;  address = inode_table_start + offset
Sector: block# = (inode# × inode_size) / block_size
        sector = (block# × block_size + inode_table_start) / sector_size
```

### Quick Reference — Write() I/O Cost
```
Appending one data block to an existing file = 5 disk I/Os:
read data bitmap → write data bitmap → read inode → write inode → write data block
```
