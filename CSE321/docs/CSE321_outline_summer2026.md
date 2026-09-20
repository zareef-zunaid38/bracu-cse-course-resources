---
source: CSE321_outline_summer2026.xlsx (the original, not included in this repo)
fidelity: the summary above the divider is paraphrased and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 0 embedded image(s); see the summary or the source)
source-mtime: 2026-06-07T07:52:04
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE 321 Summer 2026 Course Schedule & Syllabus Outline

Weekly syllabus schedule, book section mappings (Silberschatz 10th ed, MOS 4th ed, OSTEP 1.10), practice sheet links, and marks breakdown for CSE321 (Summer 2026).
Docx last modified: 07-06-2026.

## Content

### Course Information & Reference Books
- **Course Code**: CSE321
- **Course Title**: Operating Systems
- **Semester**: Summer 2026
- **Reference Books**:
  1. *Operating System Concepts* [10th ed.] (Silberschatz, Galvin, Gagne)
  2. *Modern Operating Systems* [4th ed.] (Tanenbaum)
  3. *Operating Systems - Three Easy Pieces* [1.10 ed.] (Arpaci-Dusseau)

### Weekly Lecture & Reading Schedule
| Weeks | Date Range | Topic | Book Sections (Operating System Concepts 10th ed) | Book Sections (OSTEP) | Reference Links / Files | Practice Sheets |
|---|---|---|---|---|---|---|
| **1, 2, 3** | 9 June – 22 June | Intro & Process | 3.1.1, 3.1.2, 3.1.3, 3.1.4, 3.2, 3.3, 3.4 | — | `3-1-1__3-1-2__3-1-3__3-1-4.pdf`, `3-2.pdf`, `3-3.pdf`, `3-4.pdf` | Practice Sheet_Fork |
| **3, 4** | 23 June – 29 June | Threads | 4.1, 4.2, 4.3, 4.4.1, 4.5.1, 4.6.1, 4.6.2, 4.6.3 | — | `4-1.pdf`, `4-2.pdf`, `4-3.pdf`, `4-4-1.pdf`, `4-5-1.pdf`, `4-6-1__4-6-2__4-6-3.pdf` | Practice Sheet_Threads |
| **4, 5** | 30 June – 9 July | CPU Scheduling | 5.1.1, 5.1.2, 5.1.3, 5.2, 5.3 | — | `5-1-1__5-1-2__5-1-3.pdf`, `5-2.pdf`, `5-3.pdf` | Practice Sheet_CPU Scheduling |
| **6, 7** | 11 July – 20 July | Synchronization | 6.1, 6.2, 6.4.2, 6.5, 6.6, 6.8.1, 7.1 | — | `6-1.pdf`, `6-2.pdf`, `6-4-2.pdf`, `6-5.pdf`, `6-6.pdf`, `6-8-1.pdf`, `7-1.pdf` | Practice Sheet_Process Synchronization |
| **8, 9** | 25 July – 2 August | Mid Week | — | — | — | — |
| **9, 10, 11** | 3 August – 16 August | File Systems | 11.1.1, 13.1.1, 13.1.2, 14.4.3 | Chapter 40 (File System Implementation), Chapter 42 (Crash Consistency: FSCK and Journaling) | `11-1-1.pdf`, `14-4-3.pdf`, `40.pdf`, `42.pdf` | Practice Sheet_File Systems |
| **10, 11, 12** | 12 August – 25 August | Memory Management | 9.1.1, 9.1.3, 9.3, 9.4.1, 10.1, 10.2.1, 10.2.2, 10.4.2, 10.4.3, 10.4.4 | — | `9-1-1.pdf`, `9-1-3.pdf`, `9-3.pdf`, `9-4-1.pdf`, `10-1.pdf`, `10-2-1__10-2-2.pdf`, `10-4-2__10-4-3__10-4-4.pdf` | Practice Sheet_Memory Management |
| **12, 13** | 24 August – 30 August | Protection | 17.1, 17.2, 17.4, 17.5, 17.7, 17.9, 17.10, 17.11 | — | `17-1.pdf`, `17-2.pdf`, `17-4.pdf`, `17-5.pdf`, `17-7.pdf`, `17-9.pdf`, `17-10.pdf`, `17-11.pdf` | — |
| **13, 14** | 29 August – 6 September | Security | 16.1, 16.2, 16.6.1, 16.6.2, 16.6.3, 16.6.4, 16.6.6 | — | `16-1.pdf`, `16-2.pdf`, `16-6-1__16-6-2__16-6-3__16-6-4__16-6-6.pdf` | — |
| **15, 16** | 12 September – 19 September | Final Week | — | — | — | — |

### Marks Distribution
- **Assignment**: 5.0% (Average of n)
- **Quiz**: 10.0% (Best 3 out of 4)
- **Lab**: 25.0% (Given by Lab instructors)
- **Midterm**: 25.0% (Covers Week 1 – 7)
- **Final**: 35.0% (Covers Week 9 – 14)
- **Total**: 100.0%

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: every sheet's cell values and formulas, hidden sheets marked; formatting and images are not reproduced -->

<!-- sheet: Outline -->
1: | Course Code |  | CSE321 |
2: | Course Title |  | Operating Systems |
3: | Semester |  | Summer 2026 |
4: | Reference Books: |
5: | 1. Operating System Concepts [10th ed.] |
6: | 2. Modern Operating Systems [4th ed.] |
7: | 3. Operating Systems - Three Easy Pieces [1.10 ed.] |
8: | Week | Date | Topic | Book Sections (Operating System Concepts) | Book Sections (Modern Operating Systems) | Book Sections (OSTEP) | Links to The Contents of Reference Book Sections | Practice Sheets | Quiz Schedule | Assignments |
9: | 1, 2, 3 | 9 June - 22 June | Intro |
10: |  |  | Process | 3.1.1, 3.1.2, 3.1.3, 3.1.4, 3.2, 3.3, 3.4 |  |  | 3-1-1__3-1-2__3-1-3__3-1-4.pdf | Practice Sheet_Fork |
11: |  |  |  |  |  |  | 3-2.pdf |
12: |  |  |  |  |  |  | 3-3.pdf |
13: |  |  |  |  |  |  | 3-4.pdf |
14: | 3, 4 | 23 June - 29 June | Threads | 4.1, 4.2, 4.3, 4.4.1, 4.5.1, 4.6.1, 4.6.2, 4.6.3 |  |  | 4-1.pdf | Practice Sheet_Threads |
15: |  |  |  |  |  |  | 4-2.pdf |
16: |  |  |  |  |  |  | 4-3.pdf |
17: |  |  |  |  |  |  | 4-4-1.pdf |
18: |  |  |  |  |  |  | 4-5-1.pdf |
19: |  |  |  |  |  |  | 4-6-1__4-6-2__4-6-3.pdf |
20: | 4, 5 | 30 June - 9 July | CPU Scheduling | 5.1.1, 5.1.2, 5.1.3, 5.2, 5.3 |  |  | 5-1-1__5-1-2__5-1-3.pdf | Practice Sheet_CPU Scheduling |
21: |  |  |  |  |  |  | 5-2.pdf |
22: |  |  |  |  |  |  | 5-3.pdf |
23: | 6, 7 | 11 July - 20 July | Synchronization | 6.1, 6.2, 6.4.2, 6.5, 6.6, 6.8.1, 7.1 |  |  | 6-1.pdf | Practice Sheet_Process Synchronization |
24: |  |  |  |  |  |  | 6-2.pdf |
25: |  |  |  |  |  |  | 6-4-2.pdf |
26: |  |  |  |  |  |  | 6-5.pdf |
27: |  |  |  |  |  |  | 6-6.pdf |
28: |  |  |  |  |  |  | 6-8-1.pdf |
29: |  |  |  |  |  |  | 7-1.pdf |
30: | 8, 9 | 25 July - 2 August | Mid Week |
31: | 9, 10, 11 | 3 August - 16 August | File Systems | 11.1.1, 13.1.1, 13.1.2, 14.4.3 |  | Chapter 40: (File System Implementation) Chapter 42: (Crash Consistency: FSCK and Journaling) | 11-1-1.pdf | Practice Sheet_File Systems |
32: |  |  |  |  |  |  | 14-4-3.pdf |
33: |  |  |  |  |  |  | 14-4-3.pdf |
34: |  |  |  |  |  |  | 40.pdf |
35: |  |  |  |  |  |  | 42.pdf |
36: | 10, 11, 12 | 12 August - 25 August | Memory Management | 9.1.1, 9.1.3, 9.3, 9.4.1 10.1, 10.2.1, 10.2.2, 10.4.2,10.4.3,10.4.4 |  |  | 9-1-1.pdf | Practice Sheet_Memory Management |
37: |  |  |  |  |  |  | 9-1-3.pdf |
38: |  |  |  |  |  |  | 9-3.pdf |
39: |  |  |  |  |  |  | 9-4-1.pdf |
40: |  |  |  |  |  |  | 10-1.pdf |
41: |  |  |  |  |  |  | 10-2-1__10-2-2.pdf |
42: |  |  |  |  |  |  | 10-4-2__10-4-3__10-4-4.pdf |
43: | 12, 13 | 24 August - 30 August | Protection | 17.1, 17.2, 17.4, 17.5, 17.7, 17.9, 17.10, 17.11 |  |  | 17-1.pdf |
44: |  |  |  |  |  |  | 17-2.pdf |
45: |  |  |  |  |  |  | 17-4.pdf |
46: |  |  |  |  |  |  | 17-5.pdf |
47: |  |  |  |  |  |  | 17-7.pdf |
48: |  |  |  |  |  |  | 17-9.pdf |
49: |  |  |  |  |  |  | 17-10.pdf |
50: |  |  |  |  |  |  | 17-11.pdf |
51: | 13, 14 | 29 August - 6 September | Security | 16.1, 16.2, 16.6.1, 16.6.2, 16.6.3, 16.6.4, 16.6.6 |  |  | 16-1.pdf |
52: |  |  |  |  |  |  | 16-2.pdf |
53: |  |  |  |  |  |  | 16-6-1__16-6-2__16-6-3__16-6-4__16-6-6.pdf |
54: | 15, 16 | 12 September - 19 September | Final Week |

<!-- sheet: Marks Distribution -->
1: | Marks Distribution |
2: | Assignment | Avg of n | 5.0 |
3: | Quiz | Best 3 out of 4 | 10.0 |
4: | Lab | Given by Lab instructors | 25.0 |
5: | Mid | Week 1 - 7 | 25.0 |
6: | Final | Week 9 - 14 | 35.0 |
7: | Total |  | 100.0 |
