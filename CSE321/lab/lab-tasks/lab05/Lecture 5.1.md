---
source: Lecture 5.1.pptx (kept alongside)
fidelity: the summary above the divider was rewritten from the deck on 2026-09-20 after an audit found errors in the earlier version; it is still a condensed paraphrase and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 0 embedded image(s); informational images are described in the summary)
source-mtime: 2026-08-10T05:12:30
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# Lecture 5.1: Files and Components of xv6-riscv

Lab lecture slides (10) for CSE321 Operating Systems Laboratory: an orientation to the xv6-riscv teaching OS — its purpose, top-level layout, boot sequence, system-call path, memory and file-system files, user-space files, and a quick "where to look" map. All content is text and ASCII diagrams (no images), all reproduced in the "Full extracted text" section below; this summary follows the deck's slide order.

Rewritten 2026-09-20 directly from the deck (an audit found the earlier summary added details that are not on the slides, such as the xv6 authors' names).

## Content

### Slides 1–3: Objectives and the big picture
- **Slide 2, objectives:** understand the overall structure of xv6-riscv; identify its major directories, files and components; explain at a high level how xv6 boots, creates processes, handles system calls and manages memory; relate user programs and system calls to the corresponding kernel files and layers (processes, traps, file system, devices).
- **Slide 3, user space vs kernel space:** user space holds processes (a shell process `sh` and a `cat` process); they call into the kernel via *system calls*; kernel space holds the kernel (processes, memory, files, devices, scheduler, etc.). Reference: xv6 RISC-V book, Chapter 1. *What is xv6?* A small teaching OS used in university OS courses; it reimplements an early Unix (Version 6) in modern C and RISC-V assembly; the code is intentionally small and readable. Core ideas: the kernel runs in supervisor mode and directly controls CPU, memory and devices; user programs run in user mode with restricted privileges; user programs invoke system calls to request kernel services. Link: the xv6 RISC-V book (`pdos.csail.mit.edu/6.S081/2023/xv6/book-riscv-rev3.pdf`).

### Slide 4: Top-level layout
- Tree `xv6-riscv/`: `Makefile` (build rules); `mkfs` (builds `fs.img`); `kernel/` (OS kernel, supervisor mode: `entry.S`, `start.c`, `main.c`, `proc.c`, `exec.c`, `swtch.S`, `trap.c`, `syscall.c`, `vm.c`, `kalloc.c`, `fs.c`, `file.c`, `log.c`, `bio.c`, and drivers `uart.c`, `console.c`, `virtio_disk.c`, `plic.c`, …); `user/` (user programs + tiny libc: `init.c`, `sh.c`, `ls.c`, `cat.c`, `echo.c`, …, `user.h`, `ulib.c`, `usys.S`, `printf.c`, and tests such as `usertests.c`). Reference: book Sections 2.1–2.4.
- Notes: `kernel/` holds the kernel sources (C and assembly); `user/` holds user programs (sh, ls, cat) and a tiny user-space library; `mkfs` builds the initial file-system image `fs.img` from a set of files; the `Makefile` describes how to build xv6 and its user programs and how to run xv6 under QEMU; other files are helper scripts, configuration and documentation (e.g. README).

### Slide 5: Boot path — from hardware to shell
- Hardware/QEMU (boot ROM + boot loader, outside xv6) → [1] `kernel/entry.S` — first kernel instructions in RISC-V assembly; sets up a stack and jumps to `start()` → [2] `kernel/start.c` — runs in machine mode; configures the timer and interrupt delegation, switches to supervisor mode, calls `main()` → [3] `kernel/main.c` — initialises memory, the process table, file system and devices; calls `userinit()` to start the first user program `init`; enters the scheduler loop → [4] `user/init.c` — first user program; creates the console and starts the shell `sh` → [5] `user/sh.c` — reads commands and uses `fork` and `exec` to run other programs. Reference: book Section 2.6.

### Slides 6–7: System calls, processes, memory, file system
- **Slide 6, system-call path (example `write`):** user program calls `write(fd, buf, n)` → `usys.S` (an `ECALL` with the syscall number) → `trampoline.S`, `trap.c` (trap into the kernel, identify the call) → `syscall.c` → `sys_write()` (performs the write via the file system) → return to user (`write()` returns a value). Key files: `kernel/proc.c` (process table, states, scheduler, logic for fork/exit/wait); `kernel/exec.c` (implements `exec()`, loading an ELF file into a process); `kernel/swtch.S` (context switching between processes); `kernel/sysproc.c` (process-related system calls). Reference: book Chapters 2–3.
- **Slide 7, read path:** `read(fd, buf, n)` → `sys_read()` (`sysfile.c`) → `fileread()` (`file.c`, → inode) → `readi()` (`fs.c`, → disk block numbers) → `bread()` (`bio.c`) → `virtio_disk.c` → disk. Memory files: `kernel/kalloc.c` (physical page allocator, `kalloc`/`kfree`); `kernel/vm.c` (virtual memory and page tables — maps code, data, stack, trampoline). File-system files: `kernel/fs.c`, `kernel/bio.c`, `kernel/log.c` (disk layout, buffer cache, simple log); `kernel/file.c`, `kernel/sysfile.c`, `kernel/pipe.c` (file descriptors, file-system system calls, pipes). Reference: book Chapters 8–10.

### Slides 8–10: User space, quick map, references
- **Slide 8, user program calling a system call:** a program such as `ls.c` → `user.h` + `ulib.c` (`write()`, `read()`, `open()`, …) → `usys.S` stubs (assembly `ECALL` wrapper) → kernel (`syscall.c` + `sys*.c`). Supporting code: `user.h` (prototypes and types incl. system-call declarations); `ulib.c` (basic C library functions); `printf.c` (user-space printf); `usys.S` (puts the system-call number in a register, executes `ecall`, returns the result). Important programs: `init.c` (first user program: sets up the console, starts `sh`); `sh.c` (the shell: reads commands, uses `fork()`/`exec()`, waits for them); utilities (ls, cat, echo, mkdir, rm, kill, sleep, …) and tests (`usertests`). Reference: book Chapters 1–2 and 4.
- **Slide 9, quick file mapping:** overall structure — top-level tree and `kernel/` vs `user/`; boot — `entry.S`, `start.c`, `main.c`; processes and scheduler — `proc.c`, `exec.c`, `swtch.S`; traps and system calls — `trampoline.S`, `trap.c`, `syscall.c`, `sysproc.c`, `sysfile.c`; memory and address spaces — `vm.c`, `kalloc.c`; file system and storage — `fs.c`, `bio.c`, `log.c`, `file.c`, `pipe.c`; devices and interrupts — `uart.c`, `console.c`, `plic.c`, `virtio_disk.c`; user level — `init.c`, `sh.c`, the `user/*.c` utilities, `user.h`, `ulib.c`, `usys.S`.
- **Slide 10, references:** xv6 book (RISC-V edition); the `mit-pdos/xv6-riscv` GitHub repository; MIT 6.S081 / 6.1810 Operating Systems Engineering materials; RISC-V ISA specifications (privilege levels, page tables, traps); QEMU (the emulator used to run xv6-riscv). Links: `pdos.csail.mit.edu/6.S081/`, `github.com/mit-pdos/xv6-riscv`, `riscv.org/technical/specifications/`, `www.qemu.org`.

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets; list numbering, formatting and images are not reproduced -->

<!-- slide 1 -->
### LECTURE 5.1: FILES AND COMPONENTS OF XV6-RISCV
CSE321: Operating Systems Laboratory

<!-- slide 2 -->
### OBJECTIVES
Understand the overall structure of the xv6‑riscv operating system.
Identify the major directories, files, and components of xv6‑riscv.
Explain, at a high level, how xv6 boots, creates processes, handles system calls, and manages memory.
Relate user programs and system calls to the corresponding kernel files and layers (processes, traps, file system, devices).

<!-- slide 3 -->
### BIG PICTURE: USER SPACE VS KERNEL SPACE
user space +-------------------+ +-------------------+ | shell process | | cat process | | (sh user prog) | | (user program) | +---------^---------+ +---------^---------+ | system calls | +--------------+-------------+ | kernel space v +--------------------------------------+ | Kernel | | (processes, memory, files, | | devices, scheduler, etc.) | +--------------------------------------+
Diagram Reference: xv6 RISC-V book, Chapter 1 (Overview and kernel/user split)
What is xv6?
xv6 is a small teaching operating system used in university operating systems courses. It reimplements an early version of Unix (Version 6) in modern C and RISC-V assembly. The xv6-riscv code is intentionally small and readable, which makes it suitable for students who are seeing OS internals for the first time.
Core ideas:
Kernel runs in supervisor mode and directly controls the CPU, memory, and devices.
The user programs run in user mode with restricted privileges.
User programs invoke system calls to request services from the kernel.
Links: https://pdos.csail.mit.edu/6.S081/2023/xv6/book-riscv-rev3.pdf

<!-- slide 4 -->
### TOP-LEVEL LAYOUT OF XV6-RISCV
xv6-riscv/ ├── Makefile (build rules) ├── mkfs (builds fs.img) ├── kernel/ (OS kernel, runs in supervisor mode) │ ├── entry.S │ ├── start.c │ ├── main.c │ ├── proc.c, exec.c, swtch.S │ ├── trap.c, syscall.c, ... │ ├── vm.c, kalloc.c │ ├── fs.c, file.c, log.c, bio.c, ... │ └── drivers: uart.c, console.c, virtio_disk.c, plic.c, ... └── user/ (user programs + tiny libc) ├── init.c, sh.c ├── ls.c, cat.c, echo.c, ... ├── user.h, ulib.c, usys.S, printf.c └── tests: usertests.c, etc.
Diagram Reference: xv6 RISC-V book, Sections 2.1–2.4 (Code organisation and file layout)
Important elements in the top-level directory:
kernel/ Contains the operating system kernel source files (C and assembly).
user/ Contains user programs (such as sh, ls, cat) and a tiny user‑space library.
mkfs: A tool that builds the initial xv6 file system image (fs.img) from a set of files.
Makefile: It describes how to build xv6 and its user programs, as well as how to run xv6 under QEMU.
Other files include Helper scripts, configuration files, and documentation (e.g., README).
Links: https://pdos.csail.mit.edu/6.S081/2023/xv6/book-riscv-rev3.pdf

<!-- slide 5 -->
### BOOT PATH/SEQUENCE: FROM HARDWARE TO SHELL
Hardware / QEMU | | (boot ROM + boot loader, outside xv6) v [1] entry.S (kernel/entry.S) - sets up stack - jumps to start() | v [2] start.c (kernel/start.c) - runs in machine mode - configures timer, interrupts - switches to supervisor mode - calls main() | v [3] main.c (kernel/main.c) - initialises memory, process table, file system, devices - calls userinit() -> start user program 'init' - enters scheduler loop | v [4] init (user/init.c) - creates console, starts shell 'sh' | v [5] sh (user/sh.c) - reads commands, runs other programs
Diagram Reference: xv6 RISC-V book, Section 2.6 (Starting xv6 and the first process)
Key function: A small chain of files enables xv6 to transition from raw hardware to a running shell.
kernel/entry.S: First kernel instructions, written in RISC-V assembly; sets up a stack and jumps to start().
kernel/start.c: Runs in machine mode, configures timer and interrupt delegation, switches to supervisor mode, then calls main().
kernel/main.c: Initializes memory, processes, file system, devices, starts first user process (init), and enters the scheduler.
user/init.c: First user program, sets up console and starts sh.
user/sh.c: Shell that reads commands, uses fork and exec to run programs.
Links: https://pdos.csail.mit.edu/6.S081/2023/xv6/book-riscv-rev3.pdf

<!-- slide 6 -->
### PROCESSES AND SYSTEM CALLS
User program (e.g., sh or ls) | v write(fd, buf, n) // user C call | v usys.S // ECALL with syscall number | v trampoline.S, trap.c // trap into kernel, identify syscall | v syscall.c -> sys_write() // perform write via file system | v return to user // write() returns value
Diagram Reference: xv6 RISC-V book, Chapters 2–3 (Processes and address spaces)
System call path (example: write)
Key files:
kernel/proc.c: Process table, process states, scheduler, logic for fork, exit, wait.
kernel/exec.c: Implements exec(); loads a new program (ELF file) into a process.
kernel/swtch.S: Assembly routine for context switching between processes.
kernel/sysproc.c: Kernel implementations of process-related system calls.
Links: https://pdos.csail.mit.edu/6.S081/2023/xv6/book-riscv-rev3.pdf

<!-- slide 7 -->
### MEMORY AND FILE SYSTEM (HIGH-LEVEL VIEW)
read(fd, buf, n) | v sys_read() (sysfile.c) | v fileread() (file.c) -> inode | v readi() (fs.c) -> disk block numbers | v bread() (bio.c) -> virtio_disk.c -> disk
Diagram Reference: xv6 RISC-V book, Chapters 8–10 (File system structure and logging)
Simple file system stack for read()
Memory:
kernel/kalloc.c: Physical page allocator (kalloc, kfree).
kernel/vm.c: Virtual memory and page tables (maps code, data, stack, trampoline).
File System:
kernel/fs.c, kernel/bio.c, kernel/log.c: Disk layout, buffer cache, simple log.
kernel/file.c, kernel/sysfile.c, kernel/pipe.c: File descriptors, file system system calls, pipes.
Links: https://pdos.csail.mit.edu/6.S081/2023/xv6/book-riscv-rev3.pdf

<!-- slide 8 -->
### USER‑SPACE FILES AND PROGRAMS (USER/ DIRECTORY)
user program (e.g., ls.c)     |     v user.h + ulib.c  (write(), read(), open(), ...)     |     v usys.S stubs     (assembly ECALL wrapper)     |     v kernel (syscall.c + sys*.c)
Diagram Reference: xv6 RISC-V book, Chapters 1–2 and 4 (User programs and system call interface)
User Program Calling a System Call
Supporting Code:
user.h: Contains function prototypes and type definitions for user programs, including system call declarations.
ulib.c: Implements basic C library functions for user programs.
printf.c: User-space printf implementation.
usys.S: Assembly stubs that place the system call number in a register, execute ecall, and return the result.
Important programs:
init.c: The first user program started by the kernel. It sets up the console and then starts the shell (sh).
sh.c: The shell program. It reads commands from the user, uses fork() and exec() to run other programs, and waits for them to finish.
Various utilities (ls, cat, echo, mkdir, rm, kill, sleep, etc.) and test programs (usertests).
Links: https://pdos.csail.mit.edu/6.S081/2023/xv6/book-riscv-rev3.pdf

<!-- slide 9 -->
### QUICK FILE MAPPING: WHERE TO LOOK IN XV6-RISCV
Overall structure and files: top-level tree and kernel/ vs user/
Boot: kernel/entry.S, kernel/start.c, kernel/main.c
Processes and scheduler: kernel/proc.c, kernel/exec.c, kernel/swtch.S
Traps and system calls: kernel/trampoline.S, kernel/trap.c, kernel/syscall.c, kernel/sysproc.c, kernel/sysfile.c
Memory and address spaces: kernel/vm.c, kernel/kalloc.c
File system and storage: kernel/fs.c, kernel/bio.c, kernel/log.c, kernel/file.c, kernel/pipe.c
Devices and interrupts: kernel/uart.c, kernel/console.c, kernel/plic.c, kernel/virtio_disk.c
User level: user/init.c, user/sh.c, user/*.c utilities, user/user.h, user/ulib.c, user/usys.S

<!-- slide 10 -->
### REFERENCES AND FURTHER READING
Overview of xv6-riscv design, file layout, and code examples: xv6: a simple, Unix-like teaching operating system (RISC-V edition)
Official xv6-riscv source code repository (all files mentioned here): mit-pdos/xv6-riscv on GitHub
Course materials, labs, and assignments using xv6-riscv: MIT 6.S081 / 6.1810 Operating Systems Engineering
Background on RISC-V privilege levels, page tables, and traps: RISC-V Instruction Set Architecture Specifications
Emulator used to run xv6-riscv: QEMU official website
Links: https://pdos.csail.mit.edu/6.S081/2023/xv6/book-riscv-rev3.pdf https://github.com/mit-pdos/xv6-riscv https://pdos.csail.mit.edu/6.S081/ https://riscv.org/technical/specifications/ https://www.qemu.org/
