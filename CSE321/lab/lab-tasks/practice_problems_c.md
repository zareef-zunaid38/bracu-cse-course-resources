---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# Practice Problems: Intro to C Programming

Introductory C practice problems covering numerical operations, string/file whitespace removal, password validation, email domain checking, palindrome detection with pointers, structure-based bill calculation, and perfect numbers.
Docx last modified: 27-06-2026.

## Content

### Problem 1: Basic Numerical Comparison & Arithmetic
Write a program that takes 2 numerical inputs and performs operations based on their comparison:
- If first number > second number: subtract second from first and print result.
- If first number < second number: add both and print result.
- If first number == second number: multiply both and print result.

### Problem 2: Whitespace Eraser in File
Write a program to erase multiple consecutive spaces from a text file and rewrite the cleaned sentence into another text file.
- **Sample Input**: `I      love         Python          Programming.`
- **Sample Output**: `I love Python Programming.`
- *Hint*: Split words or parse characters into a clean list/buffer.

### Problem 3: USIS Password Validator [3 Marks]
Validate a newly requested USIS password against the following security rules:
- At least one lowercase letter
- At least one uppercase letter
- At least one digit (`0-9`)
- At least one special character (`_`, `$`, `#`, `@`)

If any rule is violated, print all missing requirements (e.g., `Lowercase character missing`, `Uppercase character missing`, `Digit missing`, `Special character missing`). If valid, print `OK`.
- **Sample Inputs & Outputs**:
  - Input: `BR@CUspring` -> Output: `Digit missing`
  - Input: `bracuspring` -> Output: `Uppercase character missing, Digit missing, Special character missing`
  - Input: `BR@CU20spring22` -> Output: `OK`

### Problem 4: Sheba.xyz Email Domain Checker [3 Marks]
Write a C function that takes an employee's email ID as input and checks whether it uses the updated domain (`employeeName@sheba.xyz`) or the outdated domain (`employeeName@kaaj.com`).
- **Sample Inputs & Outputs**:
  - Input: `fahmid@kaaj.com` -> Output: `Email address is outdated`
  - Input: `zaki@sheba.xyz` -> Output: `Email address is okay`

### Problem 5: Palindrome Checker Using Pointers
Take a string input from the user and determine whether it is a palindrome using pointers (two-pointer technique).
- **Sample Inputs & Outputs**:
  - `AAABBAAA` -> `Palindrome`
  - `AABBABA` -> `Not Palindrome`
  - `aabcbaa` -> `Palindrome`

### Problem 6: Restaurant Bill Calculator Using Structures
Model restaurant order items using a C `struct` with properties `quantity` and `unit price` for three items: Paratha, Vegetable, and Mineral Water.
- Take quantity and unit price for each item from user input.
- Compute total bill.
- Take total number of people as input.
- Compute and print individual share as a float.
- **Sample Input**:
  - Quantity Of Paratha: `25`, Unit Price: `10`
  - Quantity Of Vegetables: `5`, Unit Price: `20`
  - Quantity Of Mineral Water: `20`, Unit Price: `20`
  - Number of People: `6`
- **Sample Output**:
  - `Individual people will pay: 125.00 tk` (Total = 250 + 100 + 400 = 750 / 6 = 125.00)

### Problem 7: Perfect Numbers in an Interval
Write a C program with a function to find and print all perfect numbers in a given interval $[a, b]$. (A perfect number equals the sum of its proper positive divisors).
- **Sample Input**:
  - `1`
  - `10000`
- **Sample Output**:
  - `6`
  - `28`
  - `496`
  - `8182` *(Note: in sample output text as 8182 / standard 8128)*
