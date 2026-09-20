---
source: CSE321_theory_module00_introduction.pptx (the original, included in this repo)
fidelity: the summary above the divider was rewritten from the deck on 2026-09-20 after an audit found errors in the earlier version; it is still a condensed paraphrase and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 16 embedded image(s); informational images are described in the summary)
source-mtime: 2025-06-15T19:01:26
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Module 00: Introduction

Lecture slides (16) introducing the CSE321 course and operating systems: course outcomes and marks, what an OS is, system vs application software, the goals of an OS, a timeline of operating systems, the kernel, system architecture (single-processor, multiprocessor, clustered), OS architecture (multiprogramming, time sharing), OS structure (monolithic, layered, microkernel) and OS services.
Pptx last modified: 15-06-2025.
Rewritten 2026-09-20 directly from the deck: an audit found the earlier summary's sections for slides 7–16 (Computer Startup, Interrupts, I/O Structure, Storage Hierarchy, Caching, Dual-Mode Operation, Timer) were not in this deck. Those topics are **not** covered here. Image content below was read from renders of slides 5, 7–11 and 13–16.

## Content

### Slides 1–3: Course information
- **Slide 2, course outcomes:** explore aspects of process management in an OS; know how CPU scheduling algorithms work and their importance; develop practical knowledge of threads; inspect process-synchronization mechanisms and deadlocks; analyze the management of main and virtual memory; understand file-system concepts and implementations; identify security issues and protection mechanisms.
- **Slide 3, marks distribution:** Theory 75% (Assignment 5%, Quiz 10% (n−1), Mid 25%, Final 35%); Lab 25%. Attendance needed to sit the final: 70% in theory classes, 90% in lab classes.

### Slides 4–6: What an OS is
- **Slide 4:** an OS is a program that acts as an intermediary between a user of a computer and the computer hardware. Layers: user/application (a program, application or interface) → operating system (system software) → hardware (processor, monitor, keyboard etc.).
- **Slide 5, system vs application software:** system software is the OS and all utility programs that manage computer resources at a low level (includes compilers, loaders, linkers, debuggers); application software is programs for an end user (word processors, database systems, spreadsheets). Diagram: stacked layers User – Application – Operating System – Hardware with two-way arrows.
- **Slide 6, major goals of an OS:** execute user programs; make the computer system convenient to use; use the hardware efficiently; manage and allocate all resources; control execution of user programs and operation of I/O devices.

### Slides 7–11: Timeline of OS
- **Slide 7 (timeline graphic):** 1956 GM-NAA I/O, produced by General Motors for its IBM 704; 1977 Apple II released; 1981 MS-DOS released by Microsoft; 1991 Linux released by Linus Torvalds; 1995 Windows 95 released; 2008 Android released (based on a Linux kernel); 2011 OpenShift released by Red Hat. Decade notes: 1960s — IBM develops a series of OSs for its 360 series; Multics is developed and abandoned but UNIX is developed as a consequence; 1970s — Unix becomes popular in academic circles and spawns many versions (HP-UX, AIX and others shown); 1980s — the home computer revolution; 1990s — Windows dominates the laptop and desktop market; 2000s — Unix and Linux dominate the supercomputer market; 2010s — smartphones become ubiquitous after the iPhone release in 2007.
- **Slide 8:** photo of a compact all-in-one computer being presented on stage (unlabelled; appears to be an early Macintosh). **Slide 9:** two Windows 95 screenshots (the "Welcome to Windows" dialog; a Start menu / Windows Explorer session). **Slide 10:** another revolution came in mobile computing when Steve Jobs introduced the iPhone with iOS in 2007; the iPhone introduction video is regarded as a classic advertisement; photo of the presenter beside phones of the time. **Slide 11:** "An update is available for your computer" comic (stickycomics.com): Linux — "Cool, more free stuff!", Windows — "Not again!", Mac — "Ooh, only $99!".

### Slide 12: Kernel
- The one program running at all times; the central module of an OS; the part of the OS that loads first and stays in main memory; as small as possible; provides the essential services other parts of the OS and applications need; kernel code is usually loaded into a protected area of memory so it cannot be overwritten.

### Slides 13–15: Architecture and structure (diagrams)
- **Slide 13, system architecture:** single-processor systems (applications 1–4 over the operating system over one processor), multiprocessor systems (two CPU cores, each with registers and cache, sharing memory), clustered systems (several computers linked by an interconnect to a shared storage-area network).
- **Slide 14, OS architecture:** multiprogramming (job scheduling, CPU scheduling; memory layout with the operating system at the bottom and jobs 1–4 above, up to Max) and time-sharing systems (users 1–6 around the CPU with an active link).
- **Slide 15, OS structure:** simple/monolithic (applications above a system-call interface; user space vs kernel space; memory manager, processor scheduler, inter-process communication, file system, input/output manager, network manager), layered (concentric layers from layer 0 hardware out to layer N user interface), microkernel (application program, file system and device driver in user mode exchanging messages with a microkernel that provides inter-process communication, memory management and CPU scheduling, above the hardware).

### Slide 16: Operating system services
- The OS provides an environment for executing programs; the specific services differ between operating systems but fall into common classes; they are provided for the programmer's convenience. Diagram: user and other system programs (GUI, batch, command line) → user interfaces → system calls → services (program execution, I/O operations, file systems, communication, resource allocation, accounting, error detection, protection and security) → operating system → hardware.

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets; list numbering, formatting and images are not reproduced -->

<!-- slide 1 -->
### CSE321: Operating Systems Introduction
FBA

<!-- slide 2 -->
### Course Outcome
To explore various aspects of process management in operating system
To know how different CPU scheduling algorithm works and their respective importance
To develop practical knowledge on the concepts and behaviors of threads
To inspect process synchronization mechanisms and deadlocks
To be able to analyze the management of main and virtual memory
To understand the concepts and implementations of file systems
To identify the security issues and protection mechanisms in operating systems

<!-- slide 3 -->
### Marks Distribution
Theory – 75%
  Assignment – 5%
  Quiz – 10% (n-1)
  Mid – 25%
  Final – 35%
Lab – 25%
Attendance Rule: Every student must maintain the following attendance percentages in order to attend the final exam of the course.
Theory classes: 70%
Lab classes: 90%

<!-- slide 4 -->
### What is an Operating System?
A program that acts as an intermediary
between a user of a computer and the computer hardware.
USER Application
( a program or application or interface)
Operating System
( a system software )
Hardware
( processor, monitor, keyboard etc. )

<!-- slide 5 -->
### System Software Vs Application Software
System Software:
System Software refers to the operating system and all utility programs that manage computer resources at a low level.
Systems software includes compilers, loaders, linkers, and debuggers.
Application Software:
Applications software comprises programs designed for an end user, such as word processors, database systems, and spreadsheet programs.
[image]

<!-- slide 6 -->
### Major Goals of OS
Execute user programs.
Make the computer system convenient to use.
Use the computer hardware in an efficient manner
Manages and allocate all resources
Controls the execution of user programs and operations of I/O devices

<!-- slide 7 -->
### Timeline of OS
[image]

<!-- slide 8 -->
### Timeline of OS
[image]

<!-- slide 9 -->
### Timeline of OS
[image: Windows 95]
[image: Windows 95]

<!-- slide 10 -->
### Timeline of OS
Another revolution of OS came in the mobile computing domain, when Steve Jobs introduced iPhone with iOS in 2007
The iPhone introduction video is now regarded as a classic advertise video
[image]

<!-- slide 11 -->
[image]

<!-- slide 12 -->
### Kernel
The one program running at all times.
Kernel is the central module of an operating system
Part of OS that loads first, and it remains in main memory.
As small as possible
Provide all the essential services required by other parts of the operating system and applications.
Kernel code is usually loaded into a protected area of memory to prevent it from being overwritten.

<!-- slide 13 -->
### System Architecture
Single-Processor Systems
Multiprocessor Systems
Clustered Systems
[image]
[image]
[image]

<!-- slide 14 -->
### Operating System Architecture
Multiprogramming
Job Scheduling
CPU Scheduling
Time Sharing System
[image]
[image]

<!-- slide 15 -->
### Operating System Structure
Simple/Monolithic structure
Layered structure
Microkernel structure
[image]
[image]
[image]

<!-- slide 16 -->
### Operating System Services
OS provides an environment for the execution of programs.
Specific services provided, differ from one operating system to another, but there are some common classes
Services are provided for the convenience of the programmer
[image]
