# VSD RISC-V Course Walkthrough

**Author:** Amritha S
**Institution:** VIT Chennai
**Course:** VSD RISC-V SoC Design Workshop

---

## Overview

This repository documents my completed tasks for the VSD RISC-V course. Each task builds on core RISC-V development fundamentals using open-source tools, covering the compilation pipeline, ELF analysis, and system-level setup.

---

## ✅ Task 1: RISC-V Toolchain Setup

**Objective:** Install the RISC-V GCC toolchain and verify setup.

**Commands:**

```bash
export PATH=/home/amritha-s/vsdflow/riscv/opt/riscv/bin:$PATH
echo 'export PATH=/home/amritha-s/vsdflow/riscv/opt/riscv/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
which riscv32-unknown-elf-gcc
```

**Expected Output:** Toolchain binary path confirmation.

📸 *Screenshot: Toolchain verified successfully.*

---

## ✅ Task 2: Hello World ELF Generation

**Objective:** Cross-compile a basic C program to a RISC-V ELF.

**Code (`hello.c`):**

```c
int main() {
    while(1);
    return 0;
}
```

**Command:**

```bash
riscv32-unknown-elf-gcc hello.c -o hello
```

**Expected Output:** `hello` ELF file created.

📸 *Screenshot: ELF generated from `hello.c`.*

---

## ✅ Task 3: Assembly File Generation

**Objective:** Generate the corresponding assembly from C source.

**Command:**

```bash
riscv32-unknown-elf-gcc -S hello.c -o hello.s
```

**Expected Output:** `hello.s` file with human-readable RISC-V instructions.

📸 *Screenshot: Assembly code in `hello.s`.*

---

## ✅ Task 4: Object File Creation

**Objective:** Compile C file to `.o` object format.

**Command:**

```bash
riscv32-unknown-elf-gcc -c hello.c -o hello.o
```

**Expected Output:** `hello.o` created successfully.

📸 *Screenshot: Object file created.*

---

## ✅ Task 5: Binary Disassembly

**Objective:** Use objdump to disassemble ELF.

**Command:**

```bash
riscv32-unknown-elf-objdump -d hello
```

**Expected Output:** Disassembled machine instructions visible.

📸 *Screenshot: Disassembly of `hello` ELF.*

---

## ✅ Task 6: View ELF Header

**Objective:** Inspect ELF file header metadata.

**Command:**

```bash
riscv32-unknown-elf-readelf -h hello
```

**Expected Output:** Header info including magic bytes, architecture, entry point.

📸 *Screenshot: ELF Header fields.*

---

## ✅ Task 7: View Program Headers

**Objective:** Understand the memory segments in ELF.

**Command:**

```bash
riscv32-unknown-elf-readelf -l hello
```

**Expected Output:** Program headers like LOAD segments, virtual addresses.

📸 *Screenshot: Program header table.*

---

## ✅ Task 8: Optimization Level Comparison

**Objective:** Analyze assembly differences across optimization levels.

**Commands:**

```bash
riscv32-unknown-elf-gcc -O0 -S hello.c -o hello_O0.s
riscv32-unknown-elf-gcc -O1 -S hello.c -o hello_O1.s
riscv32-unknown-elf-gcc -O2 -S hello.c -o hello_O2.s
riscv32-unknown-elf-gcc -O3 -S hello.c -o hello_O3.s
```

**Observation:** Higher `-O` levels lead to more efficient, compact code.

📸 *Screenshot: Optimization differences across levels.*

---

## ✅ Task 9: Startup Assembly File

**Objective:** Write a custom assembly file to initialize stack and jump to `main`.

**Code (`start.s`):**

```assembly
.section .text.start
.global _start
_start:
    lui sp, %hi(_stack_top)
    addi sp, sp, %lo(_stack_top)
    call main
    j .
```

📸 *Screenshot: Startup assembly structure.*

---

## ✅ Task 10: Delay Loop in `main.c`

**Objective:** Create a delay loop in a `main` C file.

**Code (`main.c`):**

```c
void delay() {
    for (volatile int i = 0; i < 100000; i++);
}

int main() {
    while (1) delay();
    return 0;
}
```

📸 *Screenshot: Main with software delay.*

---

## ✅ Task 11: Linker Script Creation

**Objective:** Define a minimal linker script.

**Code (`minimal.ld`):**

```ld
ENTRY(_start)
MEMORY {
  FLASH (rx)  : ORIGIN = 0x00000000, LENGTH = 256K
  SRAM  (rwx) : ORIGIN = 0x10000000, LENGTH = 64K
}
SECTIONS {
  .text : { *(.text.start) *(.text) *(.rodata) } > FLASH
  .data : { _data_start = .; *(.data); _data_end = .; } > SRAM
  .bss  : { _bss_start = .; *(.bss); _bss_end = .; } > SRAM
}
```

📸 *Screenshot: Linker script setup.*

---

## ✅ Task 12: Linking and Final ELF Inspection

**Objective:** Link object files using linker script and disassemble final ELF.

**Commands:**

```bash
riscv32-unknown-elf-as start.s -o start.o
riscv32-unknown-elf-gcc -c main.c -o main.o
riscv32-unknown-elf-ld start.o main.o -T minimal.ld -o task12_led_blink.elf
riscv32-unknown-elf-objdump -d task12_led_blink.elf
```

**Expected Output:** Combined executable with proper segments and symbol references.

📸 *Screenshot: Final ELF disassembly from `_start` to `main`.*

---

## 🧾 Summary

Successfully completed foundational tasks in RISC-V toolchain setup, program compilation, disassembly, and memory layout configuration using custom startup and linker scripts.

---

**Next Steps:**

* Insert all relevant screenshots
* Continue to Task 13 and onward

---

