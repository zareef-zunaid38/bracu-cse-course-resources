# Lecture 5.1: Files and Components of xv6-riscv

Slide-by-slide guide to xv6-riscv architecture, user/kernel space separation, source tree layout, boot sequence, system call pathways, memory management, file system layers, and user programs.
Docx last modified: 10-08-2026.

## Content

### Slide 1: Title
- **Course**: CSE321: Operating Systems Laboratory
- **Topic**: LECTURE 5.1: FILES AND COMPONENTS OF XV6-RISCV

### Slide 2: Objectives
- Understand the overall structure of the `xv6-riscv` operating system.
- Identify major directories, source files, and kernel components.
- Explain high-level boot flow, process creation, system call handling, and memory management.
- Map user programs and system calls to corresponding kernel source files and subsystem layers.

### Slide 3: Big Picture: User Space vs Kernel Space
- **What is xv6?**: Small Unix Version 6 teaching OS reimplemented in modern C and RISC-V assembly by MIT.
- **Core Architecture**:
  - **Supervisor Mode (Kernel)**: Direct control of CPU, physical memory, device drivers, scheduler.
  - **User Mode (User Space)**: Restricted privilege user processes (e.g. `sh`, `cat`, `ls`).
  - **Boundary**: User programs transition into kernel space via system calls (`ecall`).

### Slide 4: Top-Level Layout of xv6-riscv
```text
xv6-riscv/
├── Makefile          (build rules & QEMU launcher)
├── mkfs              (builds initial fs.img file system image)
├── kernel/           (OS kernel source, runs in supervisor mode)
│   ├── entry.S, start.c, main.c
│   ├── proc.c, exec.c, swtch.S
│   ├── trap.c, syscall.c, sysproc.c, sysfile.c
│   ├── vm.c, kalloc.c
│   ├── fs.c, file.c, log.c, bio.c, pipe.c
│   └── drivers: uart.c, console.c, virtio_disk.c, plic.c
└── user/             (user programs + minimal libc)
    ├── init.c, sh.c
    ├── ls.c, cat.c, echo.c, mkdir.c, rm.c, kill.c, sleep.c
    ├── user.h, ulib.c, usys.S, printf.c
    └── tests: usertests.c
```

### Slide 5: Boot Path / Sequence: From Hardware to Shell
1. `Hardware / QEMU`: Boot ROM loads kernel into memory at physical address `0x80000000`.
2. `kernel/entry.S`: First assembly instructions; sets up kernel stack (`stack0`) and jumps to `start()`.
3. `kernel/start.c`: Runs in M-mode (machine mode); configures timer interrupts, delegates interrupts to S-mode (supervisor mode), switches mode and calls `main()`.
4. `kernel/main.c`: Initializes physical allocator (`kinit`), page tables (`kvminit`/`kvminithart`), process table (`procinit`), trap handlers (`trapinit`/`trapinithart`), file system/disk (`binit`, `iinit`, `fileinit`, `virtio_disk_init`), calls `userinit()` to spawn first process (`init`), and invokes `scheduler()`.
5. `user/init.c`: Creates console device files (`console`), duplicates file descriptors 0, 1, 2, and spawns `sh`.
6. `user/sh.c`: Interactive shell reading user commands and invoking `fork()` / `exec()`.

### Slide 6: Processes and System Calls Pathway
- **Execution Call Flow**:
  1. User Program (e.g. `write(fd, buf, n)`)
  2. `user/usys.S`: Loads system call number `SYS_write` into register `a7`, executes `ecall`.
  3. `kernel/trampoline.S` & `kernel/trap.c`: Traps into kernel supervisor mode, saves registers, calls `usertrap()`.
  4. `kernel/syscall.c` -> `sys_write()`: Dispatches system call to corresponding implementation.
  5. Kernel returns result to user register `a0` and restores user context via `usertrapret()`.
- **Key Kernel Files**:
  - `kernel/proc.c`: Process table, process state transitions, `fork`, `exit`, `wait`, `scheduler`.
  - `kernel/exec.c`: Parses and loads ELF binaries into process address space.
  - `kernel/swtch.S`: Context switches CPU registers between thread contexts.
  - `kernel/sysproc.c`: Implementations of process-related syscalls (`sys_fork`, `sys_exit`, `sys_wait`, `sys_sleep`, `sys_kill`).

### Slide 7: Memory and File System Architecture
- **Memory Management**:
  - `kernel/kalloc.c`: Physical page allocator (allocates/frees 4096-byte pages via free list).
  - `kernel/vm.c`: Virtual memory management, page directory/table mappings, trampoline/kernel page tables.
- **File System Stack**:
  - Syscall Layer: `kernel/sysfile.c` (`sys_read`, `sys_write`, `sys_open`, etc.)
  - File Descriptor Layer: `kernel/file.c`, `kernel/pipe.c`
  - Inode / Path Layer: `kernel/fs.c` (inodes, directory lookups, block allocation)
  - Logging Layer: `kernel/log.c` (crash consistency via write-ahead logging)
  - Buffer Cache: `kernel/bio.c` (block caching and synchronization)
  - Device Driver: `kernel/virtio_disk.c` (interacts with QEMU virtual disk)

### Slide 8: User-Space Files & Support Architecture
- `user/user.h`: Prototypes for standard library and system call signatures.
- `user/ulib.c`: User-level helper implementations (`strcpy`, `strcmp`, `strlen`, `memset`, `atoi`, `gets`).
- `user/printf.c`: User-level formatted output.
- `user/usys.S`: Auto-generated assembly stubs for invoking system calls.
- `user/init.c`: Initial root process.
- `user/sh.c`: User shell program.
- Utilities & Test suites: `ls`, `cat`, `echo`, `mkdir`, `rm`, `kill`, `sleep`, `usertests`.

### Slide 9: Quick File Mapping Reference
| Subsystem | Key Files |
|---|---|
| **Boot** | `kernel/entry.S`, `kernel/start.c`, `kernel/main.c` |
| **Processes & Scheduling** | `kernel/proc.c`, `kernel/exec.c`, `kernel/swtch.S` |
| **Traps & Syscalls** | `kernel/trampoline.S`, `kernel/trap.c`, `kernel/syscall.c`, `kernel/sysproc.c`, `kernel/sysfile.c` |
| **Memory Management** | `kernel/vm.c`, `kernel/kalloc.c` |
| **File System & Storage** | `kernel/fs.c`, `kernel/bio.c`, `kernel/log.c`, `kernel/file.c`, `kernel/pipe.c` |
| **Drivers & Devices** | `kernel/uart.c`, `kernel/console.c`, `kernel/plic.c`, `kernel/virtio_disk.c` |
| **User Space** | `user/init.c`, `user/sh.c`, `user/*.c`, `user/user.h`, `user/ulib.c`, `user/usys.S` |

### Slide 10: References & Further Reading
- *xv6: a simple, Unix-like teaching operating system (RISC-V edition)* by Russ Cox, Frans Kaashoek, Robert Morris.
- MIT 6.S081 / 6.1810 Operating Systems Engineering course materials.
- Official GitHub repo: `mit-pdos/xv6-riscv`.
- RISC-V Instruction Set Architecture Specifications.
- QEMU official documentation.
