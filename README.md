![WhatsApp Image 2025-06-08 at 18 46 04_32901d8f](https://github.com/user-attachments/assets/9907efa1-c5d5-4c10-89ef-fd45749a6582)# VSD RISC-V Course Walkthrough

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
![WhatsApp Image 2025-06-08 at 17 17 20_4377cb7a](https://github.com/user-attachments/assets/c8908926-50bf-4989-8933-0206f02f7233)


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
![WhatsApp Image 2025-06-08 at 17 44 22_4f42e4d8](https://github.com/user-attachments/assets/2bcdfcd0-5e4e-482d-8c93-0fd1e5a98468)
![WhatsApp Image 2025-06-08 at 17 44 22_6e32244d](https://github.com/user-attachments/assets/694318df-9370-4708-9fdf-87a153fa5fd4)
![WhatsApp Image 2025-06-08 at 17 44 22_e12f0bab](https://github.com/user-attachments/assets/c62ad94f-c172-4164-965e-98dac2394c06)
![WhatsApp Image 2025-06-08 at 17 56 43_9c950dd5](https://github.com/user-attachments/assets/cb39a965-4c02-45bd-9af0-cc7b006ef99a)

---

## ✅ Task 3: Assembly File Generation

**Objective:** Generate the corresponding assembly from C source.

**Command:**

```bash
riscv32-unknown-elf-gcc -S hello.c -o hello.s
```

**Expected Output:** `hello.s` file with human-readable RISC-V instructions.

📸 *Screenshot: Assembly code in `hello.s`.*
![WhatsApp Image 2025-06-08 at 18 17 59_d8057492](https://github.com/user-attachments/assets/132787fc-f421-4448-958d-0f4b14c8b0e3)
![WhatsApp Image 2025-06-08 at 18 17 59_204d0e50](https://github.com/user-attachments/assets/a80316c3-e980-46b1-8a3c-3baa3d52b13f)

---

## ✅ Task 4: Object File Creation

**Objective:** Compile C file to `.o` object format.

**Command:**

```bash
riscv32-unknown-elf-gcc -c hello.c -o hello.o
```

**Expected Output:** `hello.o` created successfully.

📸 *Screenshot: Object file created.*
![WhatsApp Image 2025-06-08 at 18 29 44_ecc248f7](https://github.com/user-attachments/assets/ac87333e-7469-4e59-ae8b-039ecc748954)
![WhatsApp Image 2025-06-08 at 18 29 44_bb763304](https://github.com/user-attachments/assets/7a35de03-59ff-47cf-83ce-95f8b2894b32)

---

## ✅ Task 5: Binary Disassembly

**Objective:** Use objdump to disassemble ELF.

**Command:**

```bash
riscv32-unknown-elf-objdump -d hello
```

**Expected Output:** Disassembled machine instructions visible.

📸 *Screenshot: Disassembly of `hello` ELF.*
![WhatsApp Image 2025-06-08 at 18 38 59_11459ba6](https://github.com/user-attachments/assets/68103937-b9e9-47bf-a600-381e3d3dcea8)


---

## ✅ Task 6: View ELF Header

**Objective:** Inspect ELF file header metadata.

**Command:**

```bash
riscv32-unknown-elf-readelf -h hello
```

**Expected Output:** Header info including magic bytes, architecture, entry point.

📸 *Screenshot: ELF Header fields.*
![WhatsApp Image 2025-06-08 at 19 11 11_39d40851](https://github.com/user-attachments/assets/03a33d85-525a-4f90-a862-9f49eac98c05)
![WhatsApp Image 2025-06-08 at 19 11 11_301902b7](https://github.com/user-attachments/assets/5627f53e-1425-4352-a3be-70579214b7e3)

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
![WhatsApp Image 2025-06-08 at 19 23 05_598879bb](https://github.com/user-attachments/assets/fe59618f-8fea-4b22-91f1-78463124555f)

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
![WhatsApp Image 2025-06-08 at 19 25 12_109fa33b](https://github.com/user-attachments/assets/40d808ed-770b-40d2-a20f-e0a175e5e0a8)
![WhatsApp Image 2025-06-08 at 19 25 12_8184f607](https://github.com/user-attachments/assets/c9588b94-05b0-46ae-ba51-d881152a5498)

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
![WhatsApp Image 2025-06-08 at 19 47 07_3cd42071](https://github.com/user-attachments/assets/ee8078b4-6173-49b3-b87d-f0b9a6799066)

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

![WhatsApp Image 2025-06-08 at 19 51 35_8682b4da](https://github.com/user-attachments/assets/8e116784-82db-481d-845f-6ce8b5bf209f)
![WhatsApp Image 2025-06-08 at 20 21 48_2ecccaf7](https://github.com/user-attachments/assets/36be8732-b8f2-4afa-a8ab-336694d4615a)
![WhatsApp Image 2025-06-08 at 20 21 48_bedda2f5](https://github.com/user-attachments/assets/b98c184e-736e-4084-95ce-5212b71bc021)

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
![WhatsApp Image 2025-06-08 at 20 36 54_301220b7](https://github.com/user-attachments/assets/86cc1566-02bf-48a6-8773-21fb9533f28c)
![WhatsApp Image 2025-06-08 at 20 37 06_11441e1a](https://github.com/user-attachments/assets/49fd1ba1-a126-4f78-9ff5-db9c8322a34b)


