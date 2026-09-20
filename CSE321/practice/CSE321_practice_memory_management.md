---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# Practice Problems on Memory Management

CSE321 practice problems on paging address translation, TLB effective access time (EAT), multi-level paging hierarchies, and page replacement algorithms (FIFO, LRU, Optimal).
Docx last modified: 06-10-2025.

## Content

### Problem 1: Logical to Physical Address Translation
In a system there are 3 processes: P1 (10 bytes), P2 (6 bytes), and P3 (8 bytes) with a page size of 2 bytes ($2^1 \\implies 1\\text{ offset bit}$). Size of the main memory is 32 bytes ($2^5 \\implies 16\\text{ frames}$).

**Page Tables:**

| Process P1 (p $\\to$ f) | Process P2 (p $\\to$ f) | Process P3 (p $\\to$ f) |
| :--- | :--- | :--- |
| 0 $\\to$ 7 | 0 $\\to$ 15 | 0 $\\to$ 2 |
| 1 $\\to$ 12 | 1 $\\to$ 13 | 1 $\\to$ 6 |
| 2 $\\to$ 1 | 2 $\\to$ 9 | 2 $\\to$ 3 |
| 3 $\\to$ 5 | | 3 $\\to$ 8 |
| 4 $\\to$ 4 | | |

Find corresponding physical addresses of the following logical addresses:
- **a.** Address `01001` of P1: Page `0100` (4) $\\to$ Frame 4 (`0100`), Offset `1` $\\implies$ Physical address: `01001` = **9**
- **b.** Address `00100` of P1: Page `0010` (2) $\\to$ Frame 1 (`0001`), Offset `0` $\\implies$ Physical address: `00010` = **2**
- **c.** Address `00111` of P1: Page `0011` (3) $\\to$ Frame 5 (`0101`), Offset `1` $\\implies$ Physical address: `01011` = **11**
- **d.** Address `00001` of P2: Page `0000` (0) $\\to$ Frame 15 (`1111`), Offset `1` $\\implies$ Physical address: `11111` = **31**
- **e.** Address `00100` of P2: Page `0010` (2) $\\to$ Frame 9 (`1001`), Offset `0` $\\implies$ Physical address: `10010` = **18**
- **f.** Address `10101` of P2: Page `1010` (10) $\\implies$ **Invalid page number** (P2 has only 3 pages: 0, 1, 2)
- **g.** Address `00010` of P3: Page `0001` (1) $\\to$ Frame 6 (`0110`), Offset `0` $\\implies$ Physical address: `01100` = **12**
- **h.** Address `11011` of P3: Page `1101` (13) $\\implies$ **Invalid page number** (P3 has only 4 pages: 0, 1, 2, 3)
- **i.** Address `00110` of P3: Page `0011` (3) $\\to$ Frame 8 (`1000`), Offset `0` $\\implies$ Physical address: `10000` = **16**

#### Summary Answers:
`a. 9`, `b. 2`, `c. 11`, `d. 31`, `e. 18`, `f. Invalid page number`, `g. 12`, `h. Invalid page num.`, `i. 16`

---

### Problem 2: TLB Effective Access Time (EAT)
During TLB search, the associative lookup time ($\\epsilon$) is 3ns and hit ratio ($\\alpha$) is 70%. For each memory access, 80ns is needed. Calculate the effective access time.
$$\\text{EAT} = (\\epsilon + m)\\alpha + (\\epsilon + 2m)(1 - \\alpha) = (3 + 80) \\times 0.70 + (3 + 160) \\times 0.30 = 58.1 + 48.9 = 107\\text{ ns}$$
- **Answer**: `107ns`

---

### Problem 3: TLB Effective Access Time (EAT)
During TLB search, associative lookup time ($\\epsilon$) is 5ns and hit ratio ($\\alpha$) is 65.5%. For each memory access, 200ns is needed. Calculate the effective access time.
$$\\text{EAT} = (5 + 200) \\times 0.655 + (5 + 400) \\times 0.345 = 134.275 + 139.725 = 274\\text{ ns}$$
- **Answer**: `274ns`

---

### Problem 4: Hierarchical Paging / Two-Level Paging
Consider logical address of a process is 118 bits and the page size of the system is 16 KB ($2^{14}\\text{ Bytes} \\implies 14\\text{ offset bits}$). Size of each entry in the page table is 8 Bytes ($2^3\\text{ Bytes}$).
- Entries per page-sized page table: $\\frac{16\\text{ KB}}{8\\text{ B}} = \\frac{2^{14}}{2^3} = 2^{11} = 2048\\text{ entries} \\implies 11\\text{ bits for inner page table } (p_2)$.
- Outer page table bits: $p_1 = 118 - 14 - 11 = 93\\text{ bits}$.
- Logical address space illustration:
  `[ Outer Page (p1): 93 bits | Inner Page (p2): 11 bits | Offset (d): 14 bits ]`

---

### Problem 5: Single-Level Paging Logical to Physical Mapping
A process runs in a system with single level paging and has a logical address space of 8 bits. Page size is 16 Bytes ($2^4 \\implies 4\\text{ offset bits}$, 4 page bits $\\implies 16\\text{ pages}$). Size of main memory is 512 Bytes ($2^9 \\implies 32\\text{ frames}$).
- CPU generates logical addresses: 3, 90, 167, 241.
- **Answer**:
  - `3` (Page 0, offset 3) $\\implies$ **83**
  - `90` (Page 5, offset 10) $\\implies$ **invalid**
  - `167` (Page 10, offset 7) $\\implies$ **119**
  - `241` (Page 15, offset 1) $\\implies$ **225**

---

### Problem 6: Two-Level Paging with 8-bit Logical Address Space
A process runs with a logical address space of 8 bits. Page size is 16 Bytes (4 offset bits), size of each page table entry is 4 Bytes ($2^2$), main memory size is 512 Bytes.
- Frame capacity for page table: $\\frac{16\\text{ Bytes}}{4\\text{ Bytes}} = 4\\text{ entries} \\implies 2\\text{ bits for inner page } (p_2)$.
- Outer page bits: $p_1 = 8 - 4 - 2 = 2\\text{ bits}$.
- Breakdown: `[ Outer Page p1: 2 bits | Inner Page p2: 2 bits | Offset d: 4 bits ]`
- Mappings for logical addresses 179 and 90.

---

### Problem 7: Two-Level Paging with 16-bit Logical Address Space
A process runs with a logical address space of 16 bits. Page size is 16 Bytes (4 offset bits, 12 page bits), page table entry size is 2 Bytes, main memory is 1 KB (1024 Bytes).
- Entries per frame: $\\frac{16}{2} = 8\\text{ entries} \\implies 3\\text{ bits per level}$.
- Two-level paging hierarchy calculation until outer page table fits in one frame.
- Mappings for logical addresses 49461 and 13742.

---

### Problem 8: Page Replacement Simulation (FIFO, LRU, Optimal)
In a system with 4 frames in main memory, the process references 16 pages:
`[ 1, 5, 4, 3, 0, 4, 7, 1, 2, 9, 1, 2, 7, 3, 1, 7 ]`

#### Performance Comparison:
- **FIFO**:
  - Hit ratio: `31.25%` (5 hits / 16)
  - Fault ratio: `68.75%` (11 faults / 16)
- **LRU**:
  - Hit ratio: `37.5%` (6 hits / 16)
  - Fault ratio: `62.5%` (10 faults / 16)
- **Optimal**:
  - Hit ratio: `43.75%` (7 hits / 16)
  - Fault ratio: `56.25%` (9 faults / 16)

**Conclusion**: Optimal replacement performs best for this scenario as it achieves the lowest page fault ratio.\n