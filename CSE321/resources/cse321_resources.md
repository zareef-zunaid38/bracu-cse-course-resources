---
course: CSE321
type: resource-guide
status: complete
---

# CSE321 — Resources

## Primary Textbooks

**Operating System Concepts** — Silberschatz, Galvin, Gagne (10th ed.)
The course primary. All lecture slides map directly to its chapter/section numbers. The PDF is in HDD materials. Read the relevant sections for every topic — the questionnaire strategy explicitly says moderate-difficulty questions require reading the textbook (not just slides).

**Modern Operating Systems** — Tanenbaum (4th ed.)
Supplementary. Useful when Silberschatz explanations feel abstract. Tanenbaum writes from an implementation perspective; stronger for understanding *why* design decisions were made. PDF in HDD materials.

**Operating Systems: Three Easy Pieces (OSTEP)** — Arpaci-Dusseau (2023, v1.10)
Free at ostep.org. Mandatory for File Systems (Ch.40: VSFS implementation, Ch.42: crash consistency and journaling). Also excellent for CPU scheduling (Ch.7–9: MLFQ) and concurrency (Ch.26–34: locks, semaphores, condition variables). The most readable of the three.

**xv6: a Simple, Unix-like Teaching Operating System** — MIT (free PDF)
Required for lab component. Read alongside Lab Classes 6–7 (xv6 introduction and features, Weeks 10–11). Available at pdos.csail.mit.edu/6.1810/2023/xv6/book-riscv-rev3.pdf.

---

## Topic-by-Topic Resources

### Process (Topics 1–5)
- Silberschatz Ch.3 (sections 3.1–3.4)
- OSTEP Ch.4 (Process concept), Ch.5 (Process API — fork, exec, wait)
- **Practice:** `practice/CSE321_practice_fork.docx` (HDD) + `CSE321_practice_fork_solution.pdf` — work through all problems before midterm
- **Lab connection:** Lab 1 (System Calls, Week 3) and Lab 5 (IPC, Week 7)

### Threads (Topics 6–10)
- Silberschatz Ch.4 (sections 4.1–4.6)
- OSTEP Ch.26 (Concurrency intro), Ch.27 (Thread API)
- **Practice:** `practice/CSE321_practice_threads.docx` (HDD) — no solution provided; attempt independently
- **Lab connection:** Lab 4 (Threads and Synchronization, Week 6)

### CPU Scheduling (Topics 11–14)
- Silberschatz Ch.5 (sections 5.1–5.3)
- OSTEP Ch.7 (Scheduling: Introduction), Ch.8 (MLFQ) — MLFQ supplement used directly in slides
- **Practice:** `practice/CSE321_practice_cpu_scheduling.docx` (HDD) + `CSE321_practice_cpu_scheduling_mlfq_solution.png`
- **Key skill:** Given a process table with arrival times and burst times, draw the Gantt chart and calculate turnaround time, waiting time, and response time for all algorithms. This is the primary exam math pre-midterm.

### Synchronization (Topics 15–19)
- Silberschatz Ch.6 (sections 6.1–6.8) and Ch.7 (section 7.1)
- OSTEP Ch.28 (Locks), Ch.30 (Condition Variables), Ch.31 (Semaphores), Ch.32 (Common Concurrency Problems)
- **Practice:** `practice/CSE321_practice_synchronization.docx` (HDD) — no solution provided
- **Key skill:** Write semaphore-based solutions for the three classical problems (Bounded Buffer, Readers-Writers, Dining Philosophers). Recognize deadlock from a Resource Allocation Graph.

### File Systems (Topics 20–23)
- Silberschatz Ch.11 (11.1.1), Ch.13 (13.1.1, 13.1.2), Ch.14 (14.4.3)
- OSTEP Ch.40 (File System Implementation — VSFS), Ch.42 (Crash Consistency: FSCK and Journaling)
- **Practice:** `practice/CSE321_practice_file_systems.docx` + `CSE321_practice_file_systems_solution.docx` (HDD)
- **Key skill:** Given an inode structure, calculate the number of disk accesses needed to read a file of a given size. Understand direct, single-indirect, double-indirect blocks.

### Memory Management (Topics 24–28)
- Silberschatz Ch.9 (9.1, 9.3, 9.4.1) and Ch.10 (10.1–10.2, 10.4)
- OSTEP Ch.13 (Address Spaces), Ch.15 (Address Translation), Ch.18 (Paging), Ch.20 (TLBs), Ch.22 (Swapping)
- **Practice:** `practice/CSE321_practice_memory_management.docx` + partial solutions (`_problem4_solution.png` through `_problem7_solution.jpg`, HDD)
- **Key skill (1):** Address translation — given logical address, page size, and page table, derive the physical address.
- **Key skill (2):** Page replacement — given a reference string and frame count, trace FIFO, Optimal, and LRU to count page faults.
- **Key skill (3):** Effective access time with TLB — calculate EAT given TLB hit rate and memory access times.
- This module is the most math-dense section of the course (CO3 = 35% of course marks).

### Protection (Topics 29–30)
- Silberschatz Ch.17 (17.1, 17.2, 17.4, 17.5, 17.7, 17.9, 17.10, 17.11)
- No practice sheet. Exam questions will be theoretical (definitions, access matrix operations, comparison of MAC vs capability-based approaches).
- Moderate-difficulty questions may ask about tradeoffs between protection mechanisms not explicitly covered in slides — read the textbook sections, not only the slides.

### Security (Topics 31–32)
- Silberschatz Ch.16 (16.1, 16.2, 16.6.1–16.6.4, 16.6.6)
- No practice sheet. Conceptual understanding of threat categories and defense mechanisms.
- `slides/CSE321_textbook_map_security_notes.pdf` (HDD) is supplementary instructor notes — read it.

---

## Administrative Documents (HDD)

Base path: `Course-Library/cse/cse321/docs/`

- **`CSE321_outline_summer2026.xlsx`** — Current semester course outline (Summer 2026). Contains topic schedule, assessment dates, and CO mappings.
- **`CSE321_questionnaire_prep_strategy.docx`** — Faculty-provided exam preparation strategy. States what question difficulty levels require (slides only vs. textbook reading). Read before starting midterm prep.
- **`CSE321_course_outline.docx`** — Official OBE course outline.

## Textbook PDFs (HDD)

Base path: `Course-Library/cse/cse321/books/`

- `Silberschatz_Galvin_Gagne_Operating_System_Concepts_10e.pdf`
- `Tanenbaum_Modern_Operating_Systems_4e.pdf`
- `Arpaci-Dusseau_Operating_Systems_Three_Easy_Pieces.pdf` (OSTEP)

## Textbook-Mapped Slide Excerpts (HDD)

Base path: `Course-Library/cse/cse321/slides/CSE321_textbook_map_*.pdf` — page-scan excerpts from the reference textbooks, filed by module and keyed to Silberschatz section numbers (e.g. `CSE321_textbook_map_process_3-1-1_3-1-2_3-1-3_3-1-4.pdf`). Same numbering used in the Topic-by-Topic Resources section above. Raw theory lecture decks (PPTX) are also in `slides/` as `CSE321_theory_module0N_{topic}.pptx`.

---

## Online Tools

**CPU Scheduling Visualizer** — Use any online Gantt chart scheduler to verify your hand-traced FCFS/SJF/RR/Priority calculations before the exam. Multiple free tools available by searching "CPU scheduling simulator online."

**OS Course Slides (MIT 6.004)** — Free lecture slides that complement Silberschatz explanations, particularly for memory management and paging.
