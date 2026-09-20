---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# Practice Problems on Bash Scripting (Lab 02)

Lab 02 practice problems on Bash shell scripting covering line/word counting, file type detection, file permissions, recursive file deletion, batch copying, directory listing options, pattern matching, primes, and asterisks pattern generation.
Docx last modified: 30-06-2026.

## Content

### Problem 1: Line and Word Counter
Write a shell script that counts and prints the total number of lines and words present in a given file (e.g., using `wc -l` and `wc -w`).

### Problem 2: File Type Classifier
Write a shell script that takes a command-line argument and reports whether it is a directory (`-d`), a regular file (`-f`), or another file type.

### Problem 3: File Permission Checker
Write a shell script that verifies whether a target path is a file, and if so, checks its read (`-r`), write (`-w`), and execute (`-x`) permissions.

### Problem 4: File Deletion Across Directories
Write a shell script taking a file name as an argument and searching and deleting all occurrences of this file across directories.

### Problem 6: Batch File Copying
Write a shell script that copies multiple specified files into a target directory.

### Problem 7: Interactive Directory Listing
Write a shell script that prompts the user with 3 display options for listing files in a directory:
1. Short format display (`ls`)
2. Long format display (`ls -l`)
3. Hidden files display (`ls -a`)

### Problem 8: Pattern Match Line & Word Counter
Write a shell script that searches for a specific pattern (e.g., the word `"Hello"`) in a file and counts the number of matching lines and words.

### Problem 9: Prime Numbers Generator (20 to 70)
Write a shell script to compute and print all prime numbers between 20 and 70.

### Problem 10: Special Hollow Asterisk Pattern
Write a shell script to draw a hollow triangle pattern based on user input $N$.
- **Sample Input**: `5`
- **Sample Output**:
  ```text
      *
     * *
    *   *
   *   *
  *****
  ```

### Resources & References
- Linux Shell Scripting Tutorial: http://www.freeos.com/guides/lsst/
