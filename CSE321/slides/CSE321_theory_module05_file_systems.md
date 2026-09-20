---
source: CSE321_theory_module05_file_systems.pptx (the original, included in this repo)
fidelity: the summary above the divider was rewritten from the deck on 2026-09-20 after an audit found errors in the earlier version; it is still a condensed paraphrase and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 28 embedded image(s); informational images are described in the summary)
source-mtime: 2025-08-10T22:36:56
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Module 05: File Systems & Secondary Storage

Lecture slides (69) on hard disks and file systems: disk structure, file-system concepts, file attributes and operations, allocation methods (indexed allocation, the UNIX inode), file-system implementation in the VSFS/ext2 style (data region, inode table, bitmaps, superblock, directories, access paths), and crash consistency with fsck and journaling. **Disk-scheduling algorithms (SSTF, SCAN, etc.) are not in this deck.**
Pptx last modified: 10-08-2025.
Rewritten 2026-09-20 directly from the deck: an audit found the earlier summary's sections (disk attachment, disk scheduling, access methods, directory structures, mounting and protection) were not in this deck. Diagram, table and timeline content below was read from renders of slides 2, 5, 6, 8, 11, 13, 14, 16–18, 21–26, 28, 30–33, 38, 41, 45, 47 and 54.

## Content

### Slides 2–4: Magnetic disk structure
- **Slide 2:** hard disks provide the bulk of secondary storage and store data permanently; a hard disk consists of platter, spindle, read-write head, track and sector (Figure 1: moving-head disk mechanism — platters on a spindle, an arm assembly with read-write heads, cylinder c; Figure 2: platter, tracks and sectors).
- **Slide 3:** a disk has several circular platters, each with an upper and lower surface (both used), all attached to one spindle that rotates them together in one direction; actuator arms (read-write heads) are attached to every platter, with a separate head for each surface.
- **Slide 4:** the heads fetch data and move back and forth; the heads of both surfaces read; upper and lower surfaces have the same number of tracks; the outermost track is the external track, the innermost the internal track; to reach an inner track the head moves forward, to reach an outer track it moves backward; each track is divided into a fixed number of sectors, and data is stored in sectors.

### Slides 5–9: File-system concept, attributes, operations
- **Slide 5:** the file system is a major OS function — the software component that manages all files; different OSs use different file systems (Windows NTFS, FAT32; Mac OS Apple file system; Unix UFS; Linux ext); it determines how user data is stored in secondary memory permanently and fetched again. Figure: a possible layout — entire disk = MBR + partition table, then a disk partition containing boot block, superblock, free-space management, i-nodes, root directory, files and directories.
- **Slide 6:** all user data is managed as files (mp3, mkv, pdf, docx…); users create files in folders, modify and save them; the file system divides each file into equal-sized *logical blocks* (always a power of 2, 2ⁿ) and maps them to physical blocks/sectors on the hard disk.
- **Slide 7, file attributes (metadata):** kept in the directory structure on disk; common ones: *name* (the only human-readable one), *identifier* (unique tag, usually a number), *type/extension*, *location* (pointer to device and place on it), *size* (current and possibly maximum), *protection* (who may read, write, execute), *time, date and user identification* (creation, last modification, last use). **Slide 8:** a Mac OS X file-info window (Figure 11.1: `11.tex`, 111 KB, with kind, dates, and sharing & permissions).
- **Slide 9, operations:** creating (file with all attributes), reading, writing (edits/modifies), deleting (file and all attributes), truncating (removes information but not attributes), repositioning (moves the pointer within a file during I/O).

### Slides 10–18: File allocation methods
- **Slide 11:** after a file is created, the file system allocates it in two steps: it divides the file into equal logical blocks (2ⁿ size), then maps each logical block to physical blocks/sectors on the disk; mechanisms for this mapping are allocation methods — contiguous allocation, or non-contiguous allocation (linked allocation, indexed allocation, UNIX inode). Slide 12 is a divider "Non-Contiguous Allocation". (No detailed slide for contiguous or linked allocation appears in this deck.)
- **Slides 13–15, indexed allocation:** non-contiguous; the directory holds the file name, attributes and the index-block number; the index block holds pointers to the file's blocks; a file has one or several index blocks (multiple for very large files); information on all files' index blocks is kept in a file-allocation table in main memory. Figure: directory entry `jeep → index block 19`; index block 19 holds 9, 16, 1, 10, 25, −1, −1, −1. Benefits: supports random/direct access, no external fragmentation. Drawbacks: pointer overhead, multilevel index for large files.
- **Slide 16, UNIX inode:** a variant of indexed allocation ("i" = indexed, "node" = block) with a multilevel index; a custom structure of file attributes, direct blocks, single indirect, double indirect and triple indirect fields. Direct blocks point to data blocks; single indirect points to a block of direct pointers; double indirect to single-indirect blocks; triple indirect to double-indirect blocks. Figure: mode, owners (2), timestamps (3), size block count, direct blocks, single/double/triple indirect. **Slide 17:** the same inode with the triple-indirect tree drawn out to data blocks.
- **Slide 18, worked problem:** an inode has 4 direct block addresses, 1 single indirect, 1 double indirect and 1 triple indirect block; each block is 64 bytes and each block address 4 bytes; find the maximum file size. Solution: pointers per data block = 64 / 4 = 16; total pointers = 4 + 16 + 16² + 16³ = 4372; maximum file size = 4372 × 64 = 279,808 bytes = 273.25 KB.

### Slides 19–34: File-system implementation (VSFS, ext2-style)
- **Slide 20:** two aspects of implementing a file system — *data structures* (what on-disk structures organise data and metadata) and *access methods* (how `open()`, `read()`, `write()` map onto them, and which structures each call reads).
- **Slide 21, overall organization of VSFS (Very Simple File System, ext2):** divide the disk into 4 KB blocks addressed 0 to N−1 (figure: 64 blocks, 0–63). **Slide 22:** reserve a data region for user data (blocks 8–63 in the figure), and the file system must track which blocks make up a file, its size, owner, etc. — which raises where to store inodes. **Slide 23, inode table:** reserve blocks for an array of on-disk inodes (example: blocks 3–7, inode size 256 bytes, so a 4 KB block holds 16 inodes and the file system holds 80 inodes = the maximum number of files). **Slide 24, allocation structures:** bitmaps track free (0) or in-use (1): a *data bitmap* for the data region and an *inode bitmap* for the inode table. **Slide 25, superblock:** holds metadata about the file system (number of inodes, start of the inode table, etc.); at mount time the OS reads the superblock first. **Slide 26:** a hand-drawn "arbitrary example" of how bitmap bits (i-bmap bits 0–5, d-bmap bits 0–3) correspond to inode-table blocks and data-region blocks.
- **Slides 27–28, locating an inode:** each inode has an inode number; offset into the inode region = inode number × sizeof(inode). Example: inode 32, inode size 256 B → offset = 32 × 256 = 8192 B; inode location = inode-table start + offset = 12 KB + 8 KB = 20 KB. Disks are sector-addressable (512 B sectors): block # = (32 × 256 B) / 4 KB = 2; sector address = ((2 × 4 KB) + 12 KB) / 512 B = 40. Figure: Super (0–4 KB), i-bmap, d-bmap, then inode blocks iblock 0–4 (16 inodes each, inode 32 highlighted in iblock 2).
- **Slides 29–30, the inode:** holds all of a file's metadata — file type, size and number of blocks, protection information (owner, access), time information, etc. Slide 30 is the ext2 inode table: mode (can the file be read/written/executed), uid (owner), size (bytes), time (last accessed), ctime (created), mtime (last modified), dtime (inode deleted), gid (group), links_count (hard links), blocks (blocks allocated), flags, osd1 (OS-dependent field), **block: a set of disk pointers (15 total)**, generation (file version, used by NFS), file_acl (permissions beyond mode bits), dir_acl (access control lists), faddr (unsupported), i_osd2 (OS-dependent).
- **Slides 31–32:** hand-drawn examples: an inode's `block` field holding pointers such as 12, 18, 9 to data blocks in the data region; an inode with a single `indirect` pointer whose target block itself lists data-block pointers.
- **Slide 33, directories:** a directory is a list of (entry name, inode number) pairs, plus `.` (current directory) and `..` (parent). On-disk example for a directory with `foo`, `bar`, `foobar` — inum / reclen / strlen / name: 5 / 4 / 2 / `.`; 2 / 4 / 3 / `..`; 12 / 4 / 4 / `foo`; 13 / 4 / 4 / `bar`; 24 / 8 / 7 / `foobar`.
- **Slide 34, free-space management:** two bitmaps (inode and data) track free space; creating a file finds a free inode in the inode bitmap and updates the on-disk bitmap; a pre-allocation policy is commonly used to allocate contiguous blocks.

### Slides 35–41: Access paths
- **Slides 35–36, reading a file:** `open("/foo/bar", O_RDONLY)` traverses the path — start at the root inode (number 2 in most Unix file systems), read its block, find root's data blocks, read directory blocks to find `foo`, continue until the inode of `bar` is found, check permissions, allocate a file descriptor and return it. `read()` reads the file's first block (found via the inode), updates the inode's last-accessed time and the in-memory open-file table's offset. On `close()` the descriptor is deallocated; no disk I/O happens.
- **Slides 37–38, worked timeline** (file `bar` in 3 data blocks, path `/foo/bar`; columns: data bitmap, inode bitmap, root/foo/bar inodes, root/foo/bar[0..2] data): `open(bar)`: 1 read root inode, 2 read root data, 3 read foo inode, 4 read foo data, 5 read bar inode. First `read()`: 6 read bar inode, 7 read bar data[0], 8 write bar inode. Second `read()`: 9 read bar inode, 10 read bar data[1], 11 write bar inode. Third `read()`: 12 read bar inode, 13 read bar data[2], 14 write bar inode.
- **Slide 39, writing to disk:** `write()` updates the file with new contents and may allocate a block (unless overwriting), which means updating the data block and the data bitmap; it generates five I/Os: read the data bitmap, write the bitmap, read the inode, write the inode, write the actual block; creating a file also allocates directory space, causing high I/O traffic.
- **Slides 40–41, worked timeline** (create `/foo/bar`, then write until it occupies 3 data blocks): `create`: 1 read root inode, 2 read root data, 3 read foo inode, 4 read foo data, 5 read+write inode bitmap, 6 write foo data, 7 read+write bar inode, 8 write foo inode. First `write()`: 9 read bar inode, 10 read+write data bitmap, 11 write bar data[0], 12 write bar inode. Second `write()`: 13 read bar inode, 14 read+write data bitmap, 15 write bar data[1], 16 write bar inode. Third `write()`: 17 read bar inode, 18 read+write data bitmap, 19 write bar data[2], 20 write bar inode.

### Slides 42–69: Crash consistency and journaling
- **Slide 43:** file-system data structures (files, directories, metadata) must persist; a crash or power loss mid-update leaves the on-disk structures inconsistent; the chapter is about updating consistently (Chapter 42 of *Operating Systems: Three Easy Pieces*, Arpaci-Dusseau; the slide links a PDF).
- **Slides 44–48, the example:** appending one data block to an existing file (`open() ⇒ lseek() ⇒ write() ⇒ close()`). Before: inode I[v1] (owner remzi, read-write, size 1, one pointer to block 4), data bitmap B[v1], data block Da. After: inode I[v2] (size 2, pointers 4 and 5), bitmap B[v2], new data block Db. The file system performs three writes — data bitmap, inode, data block. Crash if only one is written: data block only → lost update; inode only → garbage/consistency problem; bitmap only → space leak. Two of three: inode + bitmap without data → consistent metadata but garbage data; inode + data without bitmap → inconsistent; bitmap + data without inode → inconsistent. This is the *crash-consistency* (consistent-update) problem.
- **Slides 49–50, fsck:** the UNIX file-system checker finds and repairs inconsistencies — superblock sanity (replace a corrupted one with an alternate copy), free blocks (scan inodes and indirect/double-indirect blocks so metadata is consistent), inode state (valid type; suspect inodes are cleared), inode links (reference counts), duplicate blocks (a block shared by two inodes), bad block pointers, and directory checks. Drawbacks: needs deep knowledge of the file system and is too slow — scanning the whole disk can take minutes to hours, prohibitive as disks grew.
- **Slides 51–52, journaling (write-ahead logging):** before overwriting on-disk structures, write a note describing the update to a log (the "write ahead"); the file system reserves space for a journal in the partition or on another device. Layout: `Super | Group 0 | … | Group N` without journaling; `Super | Journal | Group 0 | … | Group N` with it.
- **Slides 53–58, data journaling:** for the append, the transaction is `TxB | I[v2] | B[v2] | Db | TxE` (TxB = transaction begin with an id, TxE = transaction end; the blocks hold exact contents — physical logging). Steps: (1) journal write, (2) journal commit (write TxE), (3) checkpoint (write the updates to their final locations). To avoid a torn transaction, write everything except TxE first, then issue the TxE write (`TxB id=1 | I[v2] | B[v2] | Db | TxE id=1`). Recovery: if the crash is before the transaction is fully logged, the update is skipped; if it is after the commit but before the checkpoint, scan the log for committed transactions and replay them.
- **Slides 59–61, efficiency and a finite log:** batching — creating two files in one directory would log the same inode/directory block twice, so the journal keeps a global transaction written on a synchronous request or after a 5-second timeout. The log is finite (circular: Tx1 … Tx5 …); a journal superblock marks the oldest and newest transactions and which are not yet checkpointed.
- **Slides 62–63, metadata journaling (ordered journaling):** writing every data block twice is costly, so only metadata is journaled (`TxB | I[v2] | B[v2] | TxE`). Steps: data write to its final location; journal metadata write (begin + metadata); journal commit; checkpoint metadata; later, free the transaction in the journal superblock.
- **Slides 64–67, tricky case: block reuse:** revoke records. Scenario: directory `foo` is updated (journal: `TxB id=1 | I[foo] ptr:1000 | D[foo] final addr:1000 | TxE id=1`); then `foo` is deleted, freeing block 1000, and file `foobar` is created reusing block 1000 (`TxB id=2 | I[foobar] ptr:1000 | TxE id=2`). After a crash, replay would overwrite `foobar`'s user data in block 1000 with the old directory contents. Solution 1: do not reuse a deleted block until it is checkpointed. Solution 2: when deleting a directory, record a *revoke* in the journal so its metadata is not replayed.
- **Slides 68–69, timelines:** data-journaling timeline — TxB, journal contents (metadata, data) and TxE are issued and complete in order, and only then are the file-system metadata and data written (`issue`/`complete` steps in sequence); metadata-journaling timeline — the data write to the file system is issued together with the TxB/metadata journal writes, then TxE, then the metadata checkpoint. (Grid tables on the slides; see the PDF pages for the exact issue/complete ordering.)

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets; list numbering, formatting and images are not reproduced -->

<!-- slide 1 -->
### File Systems
OPERATING SYSTEMS

<!-- slide 2 -->
### Magnetic Disks/Hard Disks Structure Overview
Provide the bulk of secondary storage for modern computer systems.
Store data permanently.
Hard disk architecture consists of:
  Platter
  Spindle
  Read-write head
  Track
  Sector
[image: Diagram of a machine with text

Description automatically generated]
[image: A circular grid with numbers

Description automatically generated]
Figure 2: An illustration of platter, tracks and sectors.
Figure 1: Moving-head disk mechanism.

<!-- slide 3 -->
### Magnetic Disks/Hard Disks Structure Overview
Disk has multiple platters having circular shape like a CD.
Platters have 2 surfaces upper and lower. Both surfaces are being used.
All the platters are connected with a spindle.
Spindle rotates all platters together unidirectionally in a centric way either clockwise or anti clockwise.
When spindle moves all platters move together.
Actuator arms named read-write heads are connected with every platter.
Read-write heads are connected with both surfaces of platters. Which means a platter having upper and lower surfaces consists of two separate heads for two surfaces.

<!-- slide 4 -->
### Magnetic Disks/Hard Disks Structure Overview (cont.)
Purpose of read-write heads is fetching data.
It moves back and forth (backward and forward) in order to read data. As separate heads are connected to every surfaces of a platter that means heads can read data from both surfaces.
Upper and lower both surfaces of a platters consist of same number of multiple tracks.
The outermost track is known as external track and the innermost is known as internal track.
If the heads requires to fetch data from an inner track from its current position, then it needs to move forward.
If the heads requires to fetch data from an outer track from its current position, then it needs to move backward.
Each track (both upper and lower surface) is divided into fixed number of multiple sectors.
Data is being stored in sectors.

<!-- slide 5 -->
### File System Concept
One of the major functionalities of OS.
It is a software component of OS which manages all files.
Different OS has different file systems.
Windows has NTFS, FAT32 etc. file systems, Mac OS has apple file system (AFS), Unix has unix file system (UFS), Linux has extended (ext) file systems etc.
It determines how user data in form of files gets stored in the secondary memory or disk permanently and how these data can be fetched from the disk.
[image: A diagram of a computer component

Description automatically generated]
Figure: A possible file-system layout.

<!-- slide 6 -->
### File System Concept (cont.)

<!-- slide 7 -->
### File Attributes
Every file in the disk has a name and its data. In addition, all operating systems associate other information with each file, for example, the date and time the file was last modified and the file’s size. These information about the file is known as attributes of the file or metadata. The list of attributes varies considerably from system to system.
Information about files are kept in the directory structure, which is maintained on the disk.
Some of common attributes of files maintained by almost every OS are discussed below:
  Name: The symbolic file name is the only information kept in human readable form.
  Identifier: This unique tag, usually a number, identifies the file within the file system; it is the non-human-readable name for the file.
  Type or Extensions: This information is needed for systems that support different types of files.
  Location: This information is a pointer to a device and to the location of the file on that device.
  Size: The current size of the file (in bytes, words, or blocks) and possibly the maximum allowed size are included in this attribute.
  Protection: Access-control information determines who can do reading, writing, executing, and so on.
  Time, date, and user identification: This information may be kept for creation, last modification, and last use. These data can be useful for protection, security, and usage monitoring.

<!-- slide 8 -->
### File Attributes
[image: A screenshot of a computer

Description automatically generated]

<!-- slide 9 -->
### File Operations
Creating: Creates a file with all of its attributes or metadata.
Reading: Reads data from a file.
Writing: Edits or modifies a file.
Deleting: Removes a file alongside its all attributes.
Truncating: Removes information from a file. It does not remove attribute of the file.
Repositioning: Changes the position of the pointer within a file during IO operations.

<!-- slide 10 -->
### File Allocation Methods
OPERATING SYSTEMS

<!-- slide 11 -->
### File Allocation Methods

<!-- slide 12 -->
### Non-Contiguous Allocation
OPERATING SYSTEMS

<!-- slide 13 -->
[image]

<!-- slide 14 -->
### Indexed Allocation
[image: A diagram of a cylinder with numbers and a diagram

Description automatically generated]
Figure: Indexed allocation of disk space.

<!-- slide 15 -->
### Indexed Allocation
Benefits:
  Supports random or direct access.
  No external fragmentations.
Drawbacks:
  Pointer overhead.
  Multilevel index when file is too large.

<!-- slide 16 -->
### UNIX Inode
Variant of indexed allocation.
I means indexed and node means block.
It maintains multilevel index.
UNIX inode is a custom data structure consists of file attributes, direct blocks, single indirect, double indirect and triple indirect fields.
Direct blocks: Stores such data blocks where each block stores pointer to a block where data is stored.
Single indirect: Stores such data blocks where each block stores pointer to a direct block.
Double indirect: Stores such data blocks where each block stores pointer to a single indirect.
Triple indirect: Stores such data blocks where each block stores pointer to a double indirect.
[image: A diagram of a data flow

Description automatically generated]
Figure: The UNIX inode.

<!-- slide 17 -->
### UNIX Inode
[image: A diagram of a network

Description automatically generated]
Figure: The UNIX inode with detailed triple indirect.

<!-- slide 18 -->
[image]

<!-- slide 19 -->
### File System Implementation

<!-- slide 20 -->
### The Way to Think
There are two different aspects to implement file system
Data structures
  What types of on-disk structures are utilized by the file system to organize its data and metadata or file attributes?
Access methods
  How does it map the system calls or operations made by a process as open(), read(), write(), etc.
  Which structures are read during the execution of a particular system call?

<!-- slide 21 -->
### Overall Organization of VSFS (Very Simple File System [ext2])
Let’s develop the overall organization of the file system data structure.
Divide the disk into data blocks [data block numbers are logical or virtual addresses to the physical sectors].
  Each block size is 4 KB.
  The blocks are addressed from 0 to N -1.
[image]

<!-- slide 22 -->
### Data region in file system
Reserve data region to store user data
  File system has to track which data block comprise a file, the size of the file, its owner, etc.
How we store these inodes in file system?
[image]

<!-- slide 23 -->
### Inode table in file system
Reserve some space for inode table
  This holds an array of on-disk inodes.
  Ex) inode tables : 3 ~ 7, inode size : 256 bytes
    4-KB block can hold 16 inodes.
    The filesystem contains 80 inodes. (maximum number of files)
[image]

<!-- slide 24 -->
### Allocation Structures
This is to track whether inodes or data blocks are free or allocated.
Use bitmap, each bit indicates free(0) or in-use(1)
  data bitmap: for data region
  inode bitmap: for inode table
[image]

<!-- slide 25 -->
### Superblock
Super block contains this information for particular file system or the metadata of the file system.
  Ex) The number of inodes, begin location of inode table. etc
  Thus, when mounting a file system, OS will read the superblock first, to initialize various information.
[image]

<!-- slide 26 -->
### Dissection of the bitmaps of the file system (ext 2)
[image]

<!-- slide 27 -->
### File Organization: The inode
Each inode is referred to by inode number.
  by inode number, File system calculates where the inode is on the disk.
  Ex) inode number: 32 and size of inode: 256 Bytes
    Theorem for calculating the offset into the inode region = inode number * size of inode
      Therefore, offset in this scenario = 32 * 256 = 8192
    Byte address or location of the inode number = start address of the inode table + offset
      Therefore, location of the inode number 32 = 12 KB + 8192 B = 12 KB + 8 KB = 20 KB
    To read inode number 32, the file system would first calculate the offset into the inode region (32 sizeof(inode) or 8192), addit to the start address of the inode table on disk (inodeStartAddr = 12 KB), and thus arrive upon the correct byte address of the desired block of inodes : 20 KB.
[image]

<!-- slide 28 -->
### File Organization: The inode
Disks are not byte addressable, sector addressable.
Disks consist of a large number of addressable sectors, (512 Bytes)
  Ex) Fetch the block of inode (inode number: 32, size of inode: 256 Bytes, size of block: 4 KB)
    Sector address is the physical address of the inode block:
    block # = (inode number * sizeof(inode)) / blocksize = (32 * 256 B) / 4 KB = 8192 B / 4 KB = 8 KB / 4 KB = 2
    sector address = (block # * block size) + inode table start address ) / sector size = {(2 * 4 KB) + 12 KB} / 512 B = 40
[image]

<!-- slide 29 -->
### File Organization: The inode
inodes have all of the information or meta data or attributes of a file.
  File type (regular file, directory, etc.),
  Size, the number of blocks allocated to it.
  Protection information(who owns the file, who can access, etc).
  Time information.
  Etc.

<!-- slide 30 -->
### File Organization: The inode
[image]

<!-- slide 31 -->
### Dissection of the inode block field
[image]

<!-- slide 32 -->
### Dissection of the inode indirect
[image]

<!-- slide 33 -->
### Directory Organization
Directory contains a list of (entry name, inode number) pairs.
Each directory has two extra files .”dot” for current directory and ..”dot-dot” for parent directory
  For example, dir has three files (foo, bar, foobar)
[image]

<!-- slide 34 -->
### Free Space Management
File system track which inode and data block are free or not.
In order to manage free space, we have two simple bitmaps.
  When file is newly created, it allocated inode by searching the inode bitmap and update on-disk bitmap.
  Pre-allocation policy is commonly used for allocate contiguous blocks.

<!-- slide 35 -->
### Access Paths: Reading a File From Disk
Issue an open(“/foo/bar”, O_RDONLY)
  Traverse the pathname and thus locate the desired inode.
  Begin at the root of the file system (/)
    In most Unix file systems, the root inode number is 2
  Filesystem reads in the block that contains inode number 2.
  Look inside of it to find pointer to data blocks (contents of the root).
  By reading in one or more directory data blocks, It will find “foo” directory.
  Traverse recursively the path name until the desired inode (“bar”)
  Check finale permissions, allocate a file descriptor for this process and returns file descriptor to user.

<!-- slide 36 -->
### Access Paths: Reading a File From Disk
Issue read() to read from the file.
  Read in the first block of the file, consulting the inode to find the location of such a block.
    Update the inode with a new last accessed time.
    Update in-memory open file table for file descriptor, the file offset.
When file is closed:
  File descriptor should be deallocated, but for now, that is all the file system really needs to do. No disk I/Os take place

<!-- slide 37 -->
### Access Paths: Reading a File From Disk
Problem:
An existing file named “bar” needs to be read which is allocated in 3 data blocks.
Path of the file: “/foo/bar”
To read the file it was opened first by open() system call.
After opening the file read() system call was issued in the file to read the contents.
Illustrate the file access path timeline according to the scenario described above.

<!-- slide 38 -->
### Access Paths: Reading a File From Disk
[image]
1
2
3
4
5
6
8
9
11
12
14
7
10
13

<!-- slide 39 -->
### Access Paths: Writing to Disk
Issue write() to update the file with new contents.
File may allocate a block (unless the block is being overwritten).
  Need to update data block, data bitmap.
  It generates five I/Os:
    one to read the data bitmap
    one to write the bitmap (to reflect its new state to disk)
    two more to read and then write the inode
    one to write the actual block itself.
  To create file, it also allocate space for directory, causing high I/O traffic.

<!-- slide 40 -->
### Access Paths: Writing to Disk
Problem:
A file named “bar” has been created by create() system call.
Path of the newly created file: “/foo/bar”
After creating the file write() system call was issued in the file to write new contents and after the write operation the file has been allocated in 3 data blocks.
Illustrate the file access path timeline according to the scenario described above.

<!-- slide 41 -->
### Access Paths: Writing to Disk
[image]
1
2
3
4
6
7
8
10
9
12
11
13
16
15
19
17
20
14
18
5

<!-- slide 42 -->
### Crash Consistency and Journaling

<!-- slide 43 -->
### Overview
File system data structures must persist.
  files, directories, all of the other metadata ,etc
How to update persistent data structure?
  If the system crashes or loses power, on-disk structure will be in inconsistent state.
In this chapter, we describe how to update file system consistently
Chapter 42 from this Book: https://www.dropbox.com/scl/fi/v44z7gtbacrqlxo71brra/Remzi-H.-Arpaci-Dusseau-Andrea-C.-Arpaci-Dusseau-Operating-Systems-Three-Easy-Pieces-2023.pdf?rlkey=77bvmf6k57mrr701tryc8tj0u&dl=0

<!-- slide 44 -->
### An Example of Crash Consistency
Scenario
  Append of a single data block to an existing file.
  open() ⇒ lseek() ⇒ write() ⇒ close()
|  |  |  |  |  |  |  |  |  |  | I[v1] |  |  |  |  |  | Da |  |  |  |
|  |  |  |  | B [v1] |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
Inode
Bitmap
Data
Bitmap
Inodes
Data Blocks
Before Append a single data block
Inode Bitmap represents the number of the Inode block (0, 1, …, 7 in this example) which is filled, 1 (shaded blox) indicates full, 0 empty (blank box)
Data Bitmap represents the number of the Data block (0, 1, …, 7 in this example) which is filled, 1 (shaded blox) indicates full, 0 empty (blank box)
B[v1] represents the first version of the data block, just one single data is added
I[v1] represents the the first version of the Inode where the Inode is data structure representing a file
Da represents a data

<!-- slide 45 -->
### An Example of Crash Consistency
Scenario
  Append of a single data block to an existing file.
|  |  |  |  |  |  |  |  |  |  | I[v1] |  |  |  |  |  | Da |  |  |  |
|  |  |  |  | B [v1] |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
Inode
Bitmap
Data
Bitmap
Inodes
Data Blocks
Before Append a single data block
[image]

<!-- slide 46 -->
### An Example of Crash Consistency (Cont.)
File system perform three writes to the disk.
  Data bitmap is updated (B[v2])
  Inode is updated (I[v2])
  New Data block (Db) is added
|  |  |  |  |  |  |  |  |  |  | I[v2] |  |  |  |  |  | Da | Db |  |  |
|  |  |  |  |  | B [v2] |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
Inode
Bitmap
Data
Bitmap
Inodes
Data Blocks
After Append a single data block

<!-- slide 47 -->
### An Example of Crash Consistency (Cont.)
File system perform three writes to the disk.
  inode I[v2]
  Data bitmap B[v2]
  Data block (Db)
|  |  |  |  |  |  |  |  |  |  | I[v2] |  |  |  |  |  | Da | Db |  |  |
|  |  |  |  |  | B [v2] |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
Inode
Bitmap
Data
Bitmap
Inodes
Data Blocks
After Append a single data block
[image]

<!-- slide 48 -->
### Crash Scenario
Only one of the below block is written to disk.
  Data block (Db): lost update
  Update inode (I[v2]) block: garbage, consistency problem
  Updated bitmap (B[v2]): space leak
Two writes succeed and the last one fails.
  The inode(I[v2]) and bitmap (B[v2]), but not data (Db).: consistent from the system’s metadata
  The inode(I[v2]) and data block (Db), but not bitmap(B[v2): inconsistent
  The bitmap(B[v2]) and data block (Db), but not the inode(I[v2]): inconsistent
Crash-consistency problem (consistent-update problem)

<!-- slide 49 -->
### Solution
The File System Checker (fsck)
  fsck is a Unix tool for finding inconsistencies and repairing them.
  super block*: if the number of blocks in the filesystem is larger than the filesystem size. If the block is corrupted, the corrupted block is replaced by an alternate copy of the superblock.
  free blocks: scans the inodes, indirect blocks, double indirect blocks. Goal is to make sure the file system metadata is internally consistent.
  inode state: check if the state of each inode is valid (Ex: valid type file, directory, symbolic link, etc.). If there are problems with the inode fields that are not easily fixed. The inode is considered suspect and cleared by fsck.
  inode link: check if the reference count for each inode is consistent
  check if a block is shared by the two inodes.
  check for “bad” block pointer: ”bad” block pointer is the one that points to the location that lies outside the filesystem partition.
  directory: Check if . and .. are properly set up. Make sure that there are only one hard link for a directory.
*Super block contains this information for particular file system w Ex) The number of inodes, begin location of inode table. etc

<!-- slide 50 -->
### Drawbacks of FSCK
Building a working fsck requires complex knowledge of the file system.
fsck have a bigger and fundamental problem: too slow
  scanning the entire disk may take many minutes or hours.
  Performance of fsck became prohibitive.
    As disk grew in capacity.

<!-- slide 51 -->
### Solution
Journaling (or Write-Ahead Logging)
  Before overwriting the on-disk structures in place, write down a little note on the disk, describing what you are to do.
  Writing this note is the “write ahead”. The structure that is the destination of the “write ahead” is called log. hence, This is Write-Ahead Logging.

<!-- slide 52 -->
### Journaling
File system reserves some small amount of space within the partition or on another device.
| Super | Group 0 | Group 1 | … | Group N |  |
| Super | Journal | Group 0 | Group 1 | … | Group N |  |
without journaling
with journaling

<!-- slide 53 -->
### Data Journaling
Lets’ update a file (appending a data block to a file). Following structures are updated.
  inode (I[v2]), bitmap (B[v2]), and data block (Db)
First, Journal write: write the transaction as below.
  TxB: Transaction begin block (including transaction identifier)
  TxE: Transaction end block
  others: contain the exact contents of the blocks
| TxB | I[v2] | B[v2] | Db | TxE |  |
Journal
Transaction
physical logging

<!-- slide 54 -->
### Data Journaling (Cont.)
Second, Checkpoint: Write the physical log to their original disk locations.
| TxB | I[v2] | B[v2] | Db | TxE |  |
Journal
Transaction
checkpoint physical logging
Transaction
[image]
[image]

<!-- slide 55 -->
### Crash during Data Journaling
Wat if a crash occurs during the writes to the journal?
| TxB | I[v2] | B[v2] | ?? | TxE |  |
Journal
Transaction
physical logging

<!-- slide 56 -->
### To avoid data being inconsistent
First, write all blocks except the TxE block to journal.
Second, The file system issues the write of the TxE.
| TxB / id=1 | I[v2] | B[v2] | Db |  |
Journal
Transaction
| TxB / id=1 | I[v2] | B[v2] | Db | TxE / id=1 |  |
Journal
Transaction

<!-- slide 57 -->
### To avoid data being inconsistent (Cont.)
Journal write: write the contents of the transaction to the log
Journal commit: write the transaction commit block
Checkpoint: write the contents of the update to their locations.

<!-- slide 58 -->
### Recovery
If the crash happens, before the transaction is written to the log
  The pending update is skipped.
If the crash happens, after the transactions is written to the log, but before the checkpoint.
  Recover the update as follow:
    Scan the log and look for transactions that have committed to the disk.
    Transactions are replayed.

<!-- slide 59 -->
### Batching Log Updates
If we create two files in same directory, the same inode and the directory entry block is to the log and committed twice.
To reduce excessive write traffic to disk, journaling manage the global transaction.
  Write the content of the global transaction forced by synchronous request.
  Write the content of the global transaction after timeout of 5 seconds.

<!-- slide 60 -->
### Making the log finite
The log is of a finite size (circular log).
  To re-using it over and over
| Tx1 | Tx2 | Tx3 | Tx4 | Tx5 | … |
Journal
Transaction

<!-- slide 61 -->
### Making The log Finite (Cont.)
journal super block
  Mark the oldest and newest transactions in the log.
  The journaling system records which transactions have not been check pointed.
| Journal / Super | Tx1 | Tx2 | Tx3 | Tx4 | Tx5 | … |
Journal
Transaction

<!-- slide 62 -->
### Metadata Journaling
Because of the high cost of writing every data block to disk twice
  commit to log (journal)
  checkpoint to on-disk location.
Filesystem uses ordered journaling (metadata journaling).
| TxB | I[v2] | B[v2] | TxE |  |
Journal
Transaction
physical logging

<!-- slide 63 -->
### Metadata Journaling (Cont.)
Data Write: Write data to final location
Journal metadata write: Write the begin and metadata to the log
Journal commit: Write the transaction commit block to the log
Checkpoint metadata: Write the contents of the metadata to the disk
Free: Later, mark the transaction free in journal super block

<!-- slide 64 -->
### Tricky case: Block Reuse
Revoke record: some metadata should not be replayed.
Scenario.
  Directory “foo” is updated.
| TxB / id=1 | I[foo] / ptr:1000 | D[foo] / [final addr:1000] | TxE / id=1 |  |
Journal
Transaction
Inode of foo
Directory block of foo

<!-- slide 65 -->
### Tricky case: Block Reuse
Scenario.
  Delete “foo” directory, freeing up block 1000 for reuse
  Create a file “foobar”, reusing block 1000 for data
| TxB / id=1 | I[foo] / ptr:1000 | D[foo] / [final addr:1000] | TxE / id=1 | TxB / id=2 | I[foobar] / ptr:1000 | TxE / id=2 |  |
Journal
Transaction
Inode of foo
Directory block of foo
Inode of foobar
After crash recovery

<!-- slide 66 -->
### Tricky case: Block Reuse
Scenario.
  Now assume a crash occurs and all of this information is still in the log.
  During replay, the recovery process replays everything in the log
    Including the write of directory data in block 1000
  The replay thus overwrites the user data of current file foobar with old
  directory contents
| TxB / id= | I[foo] / ptr:1000 | D[foo] / [final addr:1000] | TxE / id=1 | TxB / id=2 | I[foobar] / ptr:1000 | TxE / id=2 |  |
Journal
Transaction
Directory block of foo
Inode of foobar
After crash recovery

<!-- slide 67 -->
### Tricky case: Block Reuse
Problem: allocate the block that was deleted but was not checkpointed.
Solution1: Do not use the deleted block until it is checkpointed.
Solution 2: When deleting a directory, record “revoke” at the journal.
| TxB / id=1 | I[foo] / ptr:1000 | D[foo] / [final addr:1000] | TxE / id=1 | TxB / id=2 | I[foobar] / ptr:1000 | TxE / id=2 |  |
Journal
Transaction
Inode of foo
Directory block of foo
Inode of foobar
After crash recovery

<!-- slide 68 -->
### Data Journaling Timeline
| TxB | Journal contents |  | TxE | File System |  |
|  | (metadata) | (data) |  | Metadata | Data |
| issue | issue | issue |  |  |  |
| complete |  |  |  |  |  |
|  | complete | complete |  |  |  |
|  |  |  | issue / complete |  |  |
|  |  |  |  | issue / complete | issue / complete |
Data Journaling Timeline

<!-- slide 69 -->
### Metadata Journaling Timeline
| TxB | Journal contents | TxE | File System |  |
|  | (metadata) |  | Metadata | Data |
| issue | issue |  |  | issue |
| complete |  |  |  |  |
|  | complete |  |  | complete |
|  |  | issue / complete |  |  |
|  |  |  | issue / complete |  |
Metadata Journaling Timeline
