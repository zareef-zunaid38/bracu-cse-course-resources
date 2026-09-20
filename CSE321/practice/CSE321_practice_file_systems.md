---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# Practice Problems on File Systems

CSE321 practice problems on UNIX inode maximum file size calculation, inode disk sector addressing, and file access path timelines for open(), read(), create(), and write().
Docx last modified: 06-10-2025.

## Content

### Problem 1: UNIX Inode Maximum File Size
A file system uses UNIX inode data structure which contains:
- 8 direct block addresses
- 2 single indirect blocks
- 2 double indirect blocks
- 2 triple indirect blocks

The size of each block is **32 Bytes** and the size of each block address is **4 Bytes**.
- **Task**: Find the maximum possible file size.

---

### Problem 2: Inode Disk & Sector Addressing
A file system has:
- Inode size = **512 Bytes**
- Block size = **4 KB**
- First 3 blocks contain superblock, data bitmap, and inode bitmap.
- Disk is sector-addressable with sector size = **256 Bytes**.

- **Tasks**:
  1. Calculate the byte address of inode number 23.
  2. Find out the sector address of the inode block containing inode 23.

---

### Problem 3: File Access Path Timeline (Read Existing File)
An existing file named `"a1"` needs to be read which is allocated in **2 data blocks**.
- Path of the file: `"/new/one/a1"`
- To read the file it was opened first by `open()` system call.
- After opening the file `read()` system call was issued on the file to read the contents.
- **Task**: Illustrate the file access path timeline according to the scenario described above across all filesystem structures (data bitmap, inode bitmap, root inode/data, intermediate directory inodes/data, file inode/data).

---

### Problem 4: File Access Path Timeline (Create and Write File)
A file named `"b1.c"` has been created by `create()` system call.
- Path of the newly created file: `"/abc/def/b1.c"`
- After creating the file `write()` system call was issued on the file to write new contents.
- After the write operation the file has been allocated in **4 data blocks**.
- **Task**: Illustrate the file access path timeline according to the scenario described above across directory traversal, inode allocation, directory updates, and data block allocation/writes.\n