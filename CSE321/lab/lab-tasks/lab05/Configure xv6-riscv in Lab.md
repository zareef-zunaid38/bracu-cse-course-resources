# Guide to Configuring xv6-riscv in Lab PCs

Step-by-step instructions for installing QEMU, the RISC-V GNU toolchain, cloning `xv6-riscv`, and running `make qemu` on lab Linux machines.
Docx last modified: 10-08-2026.

## Content

### Step 1: Checking and Installing QEMU
Check if QEMU is installed:
```bash
qemu-system-x86_64 --version
```
If not installed, run:
```bash
sudo apt update
sudo apt install qemu-kvm qemu-system-x86
```

### Step 2: Installing the RISC-V Toolchain
Install the RISC-V cross-compilers, debugger, and emulator dependencies (referencing GitHub Gist `eecsmap/00b881bbf1c652dbbb5651b4682ea0b4`):
```bash
sudo apt-get install git build-essential gdb-multiarch qemu-system-misc gcc-riscv64-linux-gnu binutils-riscv64-linux-gnu
```

### Step 3: Verifying RISC-V GCC Installation
Verify configuration:
```bash
riscv64-linux-gnu-gcc --version
```
Successful output displays the GCC version information.

### Step 4: Cloning and Running xv6-riscv
Navigate to target directory and clone the official MIT repository:
```bash
git clone https://github.com/mit-pdos/xv6-riscv.git
cd xv6-riscv
make qemu
```
