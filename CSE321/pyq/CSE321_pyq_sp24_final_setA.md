---
source: CSE321_pyq_sp24_final_setA.docx (the original, not included in this repo)
fidelity: the summary above the divider is paraphrased and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 1 embedded image(s); see the summary or the source)
source-mtime: 2025-01-31T20:04:56
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Final Examination Spring 2024 Set A

CSE321 Operating Systems Spring 2024 Set A Final exam covering circular wait mitigation in multithreaded applications, Banker's algorithm safe sequence and request verification, dynamic partitioning memory allocation (worst-fit/first-fit), TLB effective memory access time (EAT), memory fragmentation mitigation, page table logical/physical address conversion, page replacement algorithms (LRU vs OPT), disk access time calculations, and file disk allocation strategies.
Docx last modified: 31-01-2025.

## Content

### Exam Metadata
- **Institution**: BRAC University, Department of Computer Science and Engineering
- **Course**: CSE 321: Operating Systems
- **Examination**: Final Examination (Set A)
- **Semester**: Spring 2024
- **Duration**: 2 Hours
- **Total Marks**: 45

---

### Question 1 [CO4] [Marks: 3 + 4 + 4 = 11]
- **a)** Dipu is currently developing software for a multi-threaded application where threads compete for access to shared resources. However, due to performance constraints, resources cannot be preempted. Describe a situation where circular wait arises among threads and suggest strategies to mitigate deadlock. [3]
- **b)** Assume that there are five processes, P1 through P5 and 4 types of resources. At $T_0$ time we have the following system state:

  | Process | Allocation (A B C D) | Max (A B C D) | Available (A B C D) |
  | :--- | :--- | :--- | :--- |
  | **P1** | 5 2 1 1 | 8 3 3 4 | 4 6 3 0 |
  | **P2** | 5 4 3 4 | 10 8 6 6 | |
  | **P3** | 4 1 5 4 | 9 5 5 4 | |
  | **P4** | 2 1 1 2 | 2 3 4 3 | |
  | **P5** | 2 0 2 1 | 6 1 3 1 | |

  - **i.** Calculate the Need Matrix. Is this system in a safe state? If yes, then find the safe sequence using Banker's Safety algorithm; otherwise, provide the necessary explanation. [1+3]
  - **ii.** P1 requests for $(2\ 1\ 1\ 0)$, check the validity of the request. If the request is valid, does the system enter a deadlock? [1+3]

---

### Question 2 [CO5] [Marks: 5 + 2 + 3 + 3 + 3 = 16]
- **a)** At a particular time, the snapshot of the Main memory is given below for dynamic partitioning where gray portions of the memory represent occupied spaces:
  - Memory layout: `[ 300K (free) | 55K (occupied) | 675K (free) | 500K (occupied) | 230K (free) ]`
  - Apply worst fit and first fit algorithms to place processes with the space requirement of P1=50K, P2=600K, P3=250K, P4=80K, and P5=150K (in order). Explain which algorithm makes the most effective use of memory. [4+1]
- **b)** How does TLB help to shorten the effective memory access time? Explain briefly. [2]
- **c)** Compare the following scenarios and find the most efficient one in terms of the number of main memory lookups: [3]
  - **i.** $\varepsilon = 46\text{ ns}$, Memory Access Time = 100 ns, Effective Access Time = 164 ns
  - **ii.** $\varepsilon = 73\text{ ns}$, Memory Access Time = 280 ns, Effective Access Time = 395 ns
- **d)** Zahin is managing a computer lab with several PCs, each running multiple applications concurrently. Occasionally, users complain about sluggish performance and application crashes. Upon investigation, Zahin discovers that the system is experiencing memory fragmentation due to variable partitioning in main memory. Discuss how memory fragmentation can occur in this scenario and its impact on system performance. What strategies would you implement to mitigate memory fragmentation and improve system stability? [3]
- **e)** Given the page table for a process:

  | Page no | Frame no |
  | :--- | :--- |
  | 0 | 6 |
  | 1 | 11 |
  | 2 | 4 |
  | 3 | 10 |
  | 4 | 8 |
  | 5 | 9 |

  The system you are using has 48 bytes of main memory and the memory is of horizontal structure with 12 frames (frames 0 to 11, each 4 bytes). Find out the logical addresses of the following physical addresses. If the address is invalid, write "Invalid":
  - **i.** $18\ (10010_2)$
  - **ii.** $61\ (111101_2)$
  - **iii.** $27\ (11011_2)$

---

### Question 3 [CO5] [Marks: 6 + 2 + 2 = 10]
- **a)** Consider a computer with a main memory that has 4 frames and a page reference string of 1–6 pages: `[2, 1, 2, 3, 4, 5, 3, 2, 6, 1, 4, 5, 3]`. The page reference string represents the order in which the pages are accessed by a program. Apply LRU & OPT algorithm to simulate the page replacement that occurs when the main memory can hold at most 4 pages at a time. Record the number of page faults and compare the result. Mention which algorithm performs better in this scenario. [5+1]
- **b)** Explain why implementing OPT is challenging. Share an idea to overcome this challenge. [2]
- **c)** Arif Shakil is managing a server environment where multiple processes run concurrently, each requiring varying amounts of memory. However, physical memory resources are limited, leading to concerns about efficient memory management and system performance. Discuss how demand paging can address these challenges and improve system efficiency. [2]

---

### Question 4 [CO5] [Marks: 3 + 3 + 2 = 8]
- **a)** Consider, in a disk average seek time is 15 ms, disk rotation is 2000 rpm. There are 16 platters in the disk and one surface is being used. Platters have 256 tracks in each surface and 64 sectors in each track. Each sector can store 64 kb data. Find out average access time for transferring 1 GB data. [3]
- **b)** Avijit, a novice OS developer, who is unfamiliar with Unix Inode, is tasked with creating a file storage system for a digital library application. This library hosts a wide range of resources, including e-books, academic papers, research articles, and multimedia files that may get created, modified and extended regularly. Avijit must select a disk allocation strategy that ensures direct access to files and eliminates external fragmentation. Suggest the most applicable solution in this scenario with proper reasoning. [3]
- **c)** Briefly discuss the potential drawbacks of the strategy selected above. [2]

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets, headers/footers, footnotes, comments; list numbering, formatting and images are not reproduced -->

Department of Computer Science and Engineering

  (text box) A

Final Examination Spring 2024

CSE 321: Operating Systems

| Duration: 2 Hours | Total Marks: 45 |

Answer the following questions.

Figures in the right margin indicate marks.

| 1. / CO4 | a) Dipu is currently developing software for a multi-threaded application where threads compete for access to shared resources. However, due to performance constraints, resources cannot be preempted. Describe a situation where circular wait arises among threads and suggest strategies to mitigate deadlock. | [3] |

|  | b) Assume that there are five processes, P1 through P5 and 4 types of resources. At T0 time we have the following system state: / |  | Allocation |  | Max |  | Available | / |  | A | B | C | D |  | A | B | C | D |  | A | B | C | D | / | P1 | 5 | 2 | 1 | 1 |  | 8 | 3 | 3 | 4 |  | 4 | 6 | 3 | 0 | / | P2 | 5 | 4 | 3 | 4 |  | 10 | 8 | 6 | 6 |  |  |  |  |  | / | P3 | 4 | 1 | 5 | 4 |  | 9 | 5 | 5 | 4 |  |  |  |  |  | / | P4 | 2 | 1 | 1 | 2 |  | 2 | 3 | 4 | 3 |  |  |  |  |  | / | P5 | 2 | 0 | 2 | 1 |  | 6 | 1 | 3 | 1 |  |  |  |  |  | |  |

|  | i. Calculate the Need Matrix. Is this system in a safe state? If yes, then find the safe sequence using Banker’s Safety algorithm otherwise, provide the necessary explanation. | [1+3] |

|  | ii. P1 requests for (2 1 1 0), check the validity of the request. If the request is valid, does the system enter a deadlock? | [1+3] |

| 2. / CO | a) At a particular time, the snapshot of the Main memory is given below for dynamic partitioning where gray portions of the memory represent occupied spaces. Apply worst fit and first fit algorithms to place processes with the space requirement of P1=50K, P2=600K, P3=250K, P4=80K and P5=150K (in order). Explain which algorithm makes the most effective use of memory. | [4+1] |

|  | b) How does TLB help to shorten the effective memory access time? Explain briefly. | [2] |

|  | c) Compare the following scenarios and find the most efficient one in terms of the number of main memory lookups – / i. ε = 46, Memory Access Time = 100, Effective Access Time = 164 / ii. ε = 73, Memory Access Time = 280, Effective Access Time = 395 | [3] |

|  | d) Zahin is managing a computer lab with several PCs, each running multiple applications concurrently. Occasionally, users complain about sluggish performance and application crashes. Upon investigation, Zahin discovers that the system is experiencing memory fragmentation due to variable partitioning in main memory. Discuss how memory fragmentation can occur in this scenario and its impact on system performance. What strategies would you implement to mitigate memory fragmentation and improve system stability? | [3] |

|  | e) / | Page no | Frame no | / | 0 | 6 | / | 1 | 11 | / | 2 | 4 | / | 3 | 10 | / | 4 | 8 | / | 5 | 9 | / Given the page table for a process, Find out the logical addresses of the following physical addresses. If the address is invalid, write “Invalid”. / i. 18 ((10010)b) / ii. 61 ((111101)b) / iii.27 ((11011)b) / The system you are using has 48 bytes of main memory and the memory is of the following structure(horizontal). / | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | | [] |

| 3. / CO | a) Consider a computer with a main memory that has 4 frames and a page reference string of 1-6 pages: [2,1,2,3,4,5,3,2,6,1,4,5,3]. The page reference string represents the order in which the pages are accessed by a program. Apply LRU & OPT algorithm to simulate the page replacement that occurs when the main memory can hold at most 4 pages at a time. Record the number of page faults and compare the result. Mention which algorithm performs better in this scenario. | [5+1] |

|  | b) Explain why implementing OPT is challenging. Share an idea to overcome this challenge. | [2] |

|  | c) Arif Shakil is managing a server environment where multiple processes run concurrently, each requiring varying amounts of memory. However, physical memory resources are limited, leading to concerns about efficient memory management and system performance. Discuss how demand paging can address these challenges and improve system efficiency. | [2] |

| 4. / CO | a) Consider, in a disk average seek time is 15 ms, disk rotation 2000 rpm. There are 16 platters in the disk and one surface is being used. Platters have 256 tracks in each surface and 64 sectors in each track. Each sector can store 64 kb data. Find out average access time for transferring 1 GB data. | [3] |

|  | b) Avijit, a novice OS developer, who is unfamiliar with Unix Inode, is tasked with creating a file storage system for a digital library application. This library hosts a wide range of resources, including e-books, academic papers, research articles, and multimedia files that may get created, modified and extended regularly. Avijit must select a disk allocation strategy that ensures direct access to files and eliminates external fragmentation. Suggest the most applicable solution in this scenario with proper reasoning. | [3] |

|  | c) Briefly discuss the potential drawbacks of the strategy selected above. | [2] |
