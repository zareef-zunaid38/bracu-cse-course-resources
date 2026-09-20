---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# Practice Problems: Basic Shell Commands

Hands-on Linux shell command tasks covering file and directory creation, moving/copying, permissions (chmod), recursive directory manipulation, grep line counting, viewing hidden files, and line slicing.
Docx last modified: 27-06-2026.

## Content

### Problem 1: File/Directory Manipulation & Permissions Workflow
Write down the exact CLI commands in sequence for the following operations:
1. Create 4 files named `yourID_1.txt`, `yourID_2.txt`, `yourID_3`, `yourID_4.txt` and 2 directories named `YourName1`, `YourName2`.
2. Move `yourID_1.txt`, `yourID_2.txt`, `yourID_3.txt` into `YourName1/`.
3. Copy `yourID_1.txt`, `yourID_2.txt` into `YourName2/`.
4. Create another directory `YourName3/`.
5. Copy directory `YourName1/` along with its contents into `YourName3/` (e.g., `cp -r YourName1 YourName3/`).
6. Navigate into `YourName3/`, check permissions of files/directories (`ls -l`), and change permissions for both group and others to read-execute only (`chmod go=rx *` or `chmod go=rx,go-w *`).
7. Navigate up one directory (`cd ..`), list all files and directories in current working directory (`ls -la`), move `YourName3/` to root directory (`/`), and delete all remaining files and directories in current working directory.

### Problem 2: Course Information File Filtering
Create a file named `course.txt` containing current semester course information (course IDs, names, sections, etc.). Count the number of lines containing the substring `"CSE"` in `course.txt` (e.g. `grep -c "CSE" course.txt`).

### Problem 3: Hidden Files Display
Show all hidden files located in the root directory (`ls -a /` or `ls -d /.*`).

### Problem 4: Line Slicing from File
Create a text file containing more than 17 lines and display only lines 5 through 17 (e.g., `sed -n '5,17p' file.txt` or `head -n 17 file.txt | tail -n 13`).
