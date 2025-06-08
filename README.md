# RISC-V Toolchain Tasks - Complete Guide

This repository contains a comprehensive set of 17 tasks designed to verify and demonstrate the RISC-V cross-compilation toolchain functionality. Each task builds upon the previous ones, progressing from basic compilation to advanced embedded systems programming concepts.

## Table of Contents

- [Task 1: Basic Hello World Compilation](#task-1-basic-hello-world-compilation)
- [Task 2: Build Full RISC-V Executable](#task-2-build-full-risc-v-executable)
- [Task 3: Disassemble and Inspect ELF](#task-3-disassemble-and-inspect-elf)
- [Task 5: Compile Assembly Code](#task-5-compile-assembly-code)
- [Task 6: Combine Assembly and C Code](#task-6-combine-assembly-and-c-code)
- [Task 7: Inspect Symbols](#task-7-inspect-symbols)
- [Task 8: Compiler Optimizations](#task-8-compiler-optimizations)
- [Task 9: Understanding Optimization with objdump](#task-9-understanding-optimization-with-objdump)
- [Task 10: Debug Info and Symbol Analysis](#task-10-debug-info-and-symbol-analysis)
- [Task 11: Custom Linker Script](#task-11-custom-linker-script)
- [Task 12: LED Blinking with Custom Linker](#task-12-led-blinking-with-custom-linker)
- [Task 13: Interrupt Handling (MTIP)](#task-13-interrupt-handling-mtip)
- [Task 14: RV32IMAC vs RV32IMC Comparison](#task-14-rv32imac-vs-rv32imc-comparison)
- [Task 15: Atomic Operations Test](#task-15-atomic-operations-test)
- [Task 16: Newlib printf on Bare-Metal](#task-16-newlib-printf-on-bare-metal)
- [Task 17: Endianness and Struct Packing](#task-17-endianness-and-struct-packing)

---

## Task 1: Basic Hello World Compilation

### 🎯 Objective
Verify that your RISC-V cross compiler works by compiling a simple C program (hello.c) into an object file (hello.o) and checking that the toolchain runs successfully without errors.

### 📜 C Code - hello.c
```c
#include <stdio.h>

int main() {
    printf("Hello, VSD!\n");
    return 0;
}
```

### 📁 File Location
```bash
~/Desktop/vsdflow/task1/hello.c
```

### ⚙️ Working Commands
```bash
cd ~/Desktop/vsdflow/task1

# Compile hello.c to RISC-V object code
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-gcc -c hello.c -o hello.o

# Check that object file is created
ls -lh hello.o
```

### Expected Output
```bash
-rw-r--r-- 1 amritha-s amritha-s 1.2K ... hello.o
```

### 📋 Task Checklist
| Task # | Description | Done? |
|--------|-------------|-------|
| 1.1 | Write a simple C program (hello.c) | ✅ |
| 1.2 | Use riscv32-unknown-elf-gcc to compile it | ✅ |
| 1.3 | Verify hello.o is generated | ✅ |
| 1.4 | Capture a screenshot of output in terminal | 🔲 |

### 🖼️ Output Screenshot
📸 Paste your screenshot below this line showing the terminal output after running the commands.

### 📘 Explanation
In embedded systems and compiler verification, it's important to start small and check that:
- ✅ The compiler runs correctly
- ✅ The input source compiles to an object file
- ✅ The object file contains valid RISC-V machine code

---

## Task 2: Build Full RISC-V Executable

### 🎯 Objective
Take the compiled object file from hello.c and link it into an actual RISC-V executable (hello.elf) using riscv32-unknown-elf-gcc. This tests the ability of the toolchain to link and generate a complete ELF binary.

### 📁 File Structure
```bash
~/Desktop/vsdflow/task2/
├── hello.c
```

### Prerequisites
If hello.c doesn't exist, copy it from Task 1:
```bash
cp ~/Desktop/vsdflow/task1/hello.c ~/Desktop/vsdflow/task2/
```

### 💻 Working Commands
```bash
cd ~/Desktop/vsdflow/task2

# Step 1: Compile C code into object file
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-gcc -c hello.c -o hello.o

# Step 2: Link object file into full ELF executable
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-gcc hello.o -o hello.elf

# Step 3: Check that ELF file is created
ls -lh hello.elf
```

### Expected Output
```bash
-rwxr-xr-x 1 amritha-s amritha-s 7.5K ... hello.elf
```

### 📋 Task Checklist
| Task # | Description | Done? |
|--------|-------------|-------|
| 2.1 | Copy or reuse hello.c from Task 1 | ✅ |
| 2.2 | Compile to object file: hello.o | ✅ |
| 2.3 | Link to ELF executable: hello.elf | ✅ |
| 2.4 | Confirm ELF file is created via ls -lh hello.elf | ✅ |
| 2.5 | Capture screenshot of ELF generation in terminal | 🔲 |

### 🖼️ Output Screenshot
📸 Paste your terminal screenshot here showing the ELF file (hello.elf) successfully created.

### 📘 Explanation
Linking is the second key step in the embedded toolchain pipeline:
- hello.c is first compiled into hello.o (raw code + symbol info)
- Then hello.o is linked to generate hello.elf — a proper executable with:
  - Program headers
  - Sections like .text, .data, .bss
  - Entry point (_start or main)
- hello.elf can now be inspected, simulated, or flashed (later)

---

## Task 3: Disassemble and Inspect ELF

### 🎯 Objective
Disassemble the hello.elf binary using objdump to verify what machine instructions were generated by the RISC-V toolchain for your C code. This step proves that the compiler is working and that you're able to interpret generated RISC-V assembly.

### 📁 File Structure
```bash
~/Desktop/vsdflow/task3/
├── hello.c         # (Copied from Task 1 or 2)
├── hello.o         # (Compiled)
├── hello.elf       # (Linked ELF file)
```

### Prerequisites
If you don't have these files yet:
```bash
cp ~/Desktop/vsdflow/task2/* ~/Desktop/vsdflow/task3/
```

### 💻 Working Commands
```bash
cd ~/Desktop/vsdflow/task3

# Step 1: Compile C code if needed
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-gcc -c hello.c -o hello.o

# Step 2: Link to generate ELF
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-gcc hello.o -o hello.elf

# Step 3: Disassemble the ELF to view RISC-V instructions
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-objdump -d hello.elf | less
```

**Note:** Press `q` to quit the disassembly viewer (less).

### 📋 Task Checklist
| Task # | Description | Done? |
|--------|-------------|-------|
| 3.1 | Ensure hello.elf is available from previous task | ✅ |
| 3.2 | Use objdump to disassemble the ELF | ✅ |
| 3.3 | Confirm function labels and assembly instructions appear | ✅ |
| 3.4 | Take a screenshot of the disassembled output in terminal | 🔲 |

### 🖼️ Output Screenshot
📸 Paste screenshot showing assembly output — particularly the disassembly of main function.

### Expected Disassembly Example
```asm
00000074 <main>:
  ...
  00000078:   00000513    li a0,0
  0000007c:   00008067    ret
```

### 📘 Explanation
This task validates:
- ✅ That your compiled ELF contains valid RISC-V machine code
- ✅ That the toolchain correctly emits labels like main, _start
- ✅ That objdump -d allows you to inspect what instructions were generated

This is crucial for debugging low-level behavior and ensuring what your source code compiles into at the ISA level.

---

## Task 5: Compile Assembly Code

### 🎯 Objective
Compile a standalone RISC-V assembly file (start.s) and link it into an ELF file using the RISC-V toolchain. This verifies that the assembler and linker stages work independently of C code.

### 📁 File Structure
```bash
~/Desktop/vsdflow/task5/
├── start.s         # Simple RISC-V assembly file
├── start.o         # Assembled object file
├── start.elf       # Final linked ELF file
```

### 💻 Working Commands
```bash
cd ~/Desktop/vsdflow/task5

# Step 1: Create your assembly file (if not yet done)
cat << 'EOF' > start.s
.section .text
.global _start
_start:
    li a0, 10
    li a1, 20
    add a2, a0, a1
1:  j 1b
EOF

# Step 2: Assemble to object file
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-as start.s -o start.o

# Step 3: Link into an ELF
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-ld start.o -o start.elf

# Step 4: Verify ELF sections
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-objdump -d start.elf | less
```

### 📋 Task Checklist
| Task # | Description | Done? |
|--------|-------------|-------|
| 5.1 | Create standalone start.s assembly file | ✅ |
| 5.2 | Assemble to start.o using as | ✅ |
| 5.3 | Link to start.elf using ld | ✅ |
| 5.4 | Verify machine instructions via objdump | ✅ |
| 5.5 | Screenshot of assembly + ELF disassembly | 🔲 |

### 🖼️ Output Screenshot
📸 Take a screenshot showing successful assembly, linking, and output from:
```bash
riscv32-unknown-elf-objdump -d start.elf
```

### 📘 Explanation
This task proves:
- ✅ You can write, assemble, and link raw RISC-V assembly
- ✅ The ELF file structure is correct even without C runtime
- ✅ Understanding of label _start, registers, and loops

---

## Task 6: Combine Assembly and C Code

### 🎯 Objective
Link an assembly startup file (start.s) with a simple C program (hello.c) to form a working ELF, mimicking how embedded programs boot from assembly and jump into C.

### 📁 File Structure
```bash
~/Desktop/vsdflow/task6/
├── start.s         # Assembly file with _start
├── hello.c         # Simple C function (no main needed)
├── start.o         # Object from start.s
├── hello.o         # Object from hello.c
├── start.elf       # Final linked ELF
```

### 💻 Working Commands
```bash
cd ~/Desktop/vsdflow/task6

# Step 1: Write hello.c
cat << 'EOF' > hello.c
int my_function() { return 42; }
EOF

# Step 2: Reuse or write start.s
cat << 'EOF' > start.s
.section .text
.global _start
_start:
    call my_function
1:  j 1b
EOF

# Step 3: Compile both
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-gcc -c hello.c -o hello.o
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-as start.s -o start.o

# Step 4: Link them
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-ld start.o hello.o -o start.elf

# Step 5: Disassemble to confirm function linkage
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-objdump -d start.elf | less
```

### 📋 Task Checklist
| Task # | Description | Done? |
|--------|-------------|-------|
| 6.1 | Create C function (hello.c) | ✅ |
| 6.2 | Call C function from _start in assembly | ✅ |
| 6.3 | Compile both .c and .s to .o files | ✅ |
| 6.4 | Link them to start.elf | ✅ |
| 6.5 | Disassemble to confirm call my_function | ✅ |
| 6.6 | Screenshot with function call disassembly | 🔲 |

### 🖼️ Output Screenshot
Show a disassembly with the call my_function inside _start, plus the function itself.

### 📘 Explanation
You've built an ELF where:
- 🧠 Assembly handles boot (simulating reset vector)
- 🧠 C logic handles logic (like embedded apps do)
- 🧠 You verified the function call path manually via disassembly

---

## Task 7: Inspect Symbols

### 🎯 Objective
Use nm and objdump to explore symbols in the ELF — both from C and Assembly — and understand where functions and data reside.

### 📁 File Structure
```bash
~/Desktop/vsdflow/task7/
├── hello.c         # Your C code from Task 6
├── start.s         # Assembly file with _start
├── *.o             # Object files
├── start.elf       # Final executable
```

### 💻 Working Commands
```bash
cd ~/Desktop/vsdflow/task7

# Copy working files from Task 6
cp ~/Desktop/vsdflow/task6/* .

# Rebuild for clean state
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-gcc -c hello.c -o hello.o
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-as start.s -o start.o
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-ld start.o hello.o -o start.elf

# View symbols
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-nm start.elf
```

### Expected Output
```bash
00000000 T _start
00000014 T my_function
```

### 📋 Task Checklist
| Task # | Description | Done? |
|--------|-------------|-------|
| 7.1 | Use nm to inspect function symbols | ✅ |
| 7.2 | Confirm _start and my_function locations | ✅ |
| 7.3 | Explain sections (T = text, D = data, B = bss) | ✅ |
| 7.4 | Screenshot of nm output | 🔲 |

### 🖼️ Output Screenshot
Take screenshot of nm start.elf output showing symbols:
```
00000000 T _start
00000014 T my_function
```

### 📘 Explanation
You now understand:
- ✅ What symbols are in an ELF file
- ✅ How to locate your functions (_start, main, etc.)
- ✅ How code gets organized into sections (.text, .data, .bss)

---

## Task 8: Compiler Optimizations

### 🎯 Objective
Compare performance and size differences of the compiled RISC-V code under different GCC optimization flags (-O0, -O1, -O2, -O3) using a loop delay program.

### 📁 File Setup
```bash
cd ~/Desktop/vsdflow && mkdir -p task8 && cd task8
```

### 💻 Code - loop_opt.c
```c
void delay_loop() {
    volatile int i;
    for (i = 0; i < 10000; i++) {
        // Do nothing
    }
}

int main() {
    delay_loop();
    return 0;
}
```

### 💻 Build Script - build_all_opts.sh
```bash
#!/bin/bash
RISCV=/home/amritha-s/vsdflow/riscv/opt/riscv/bin

for opt in 0 1 2 3; do
    echo "=== Building with -O$opt ==="
    $RISCV/riscv32-unknown-elf-gcc -O$opt -o loop_O$opt.elf loop_opt.c
    $RISCV/riscv32-unknown-elf-size loop_O$opt.elf
    echo ""
done
```

### Execute Build
```bash
chmod +x build_all_opts.sh
./build_all_opts.sh
```

### 📋 Explanation
This builds the same file with four optimization levels:
- **-O0**: No optimization (baseline)
- **-O1**: Basic optimization
- **-O2**: Further optimization including loop unrolling
- **-O3**: Aggressive optimization (may inline)

### 📊 Summary Table
| Flag | Optimizes For | Binary Size | Speed (Subjective) |
|------|---------------|-------------|-------------------|
| -O0 | Debugging, no opt | Largest | Slowest |
| -O1 | Basic perf | Smaller | Faster |
| -O2 | Balance perf/size | Smaller | Fast |
| -O3 | Max speed | May be same | Fastest |

### 🖼️ Output Screenshot
📸 Screenshot of terminal running ./build_all_opts.sh and showing sizes

---

## Task 9: Understanding Optimization with objdump

### 🎯 Objective
Disassemble .elf files to compare assembly instructions generated at each optimization level.

### 💻 Disassemble with objdump
```bash
for opt in 0 1 2 3; do
    echo "=== Disassembly with -O$opt ==="
    /home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-objdump -d loop_O$opt.elf > disasm_O$opt.txt
done
```

Then inspect each `disasm_Ox.txt` to compare loop code and check if:
- Loop is optimized
- Function is inlined
- NOPs removed

### 📋 Explanation
Using objdump -d, you decode machine code into human-readable assembly to understand compiler behavior.

### 🖼️ Output Screenshot
📸 Screenshot of one of the disassembled .txt files or diff comparison

---

## Task 10: Debug Info and Symbol Analysis

### 🎯 Objective
Use -g flag to include debug info and explore .symtab.

### 💻 Compile with Debug Info
```bash
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-gcc -O2 -g -o debug.elf loop_opt.c
```

### 💻 View Symbols with nm
```bash
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-nm debug.elf
```

### 📋 Explanation
With -g, symbols like main, delay_loop remain visible. Use nm to confirm variable locations, function scope, and linkage.

### 🖼️ Output Screenshot
📸 Screenshot of output from nm debug.elf

---

## Task 11: Custom Linker Script

### 🎯 Objective
Write a minimal custom linker script to control memory layout. Specifically:
- Place `.text` in Flash memory (0x00000000)
- Place `.data` and `.bss` in SRAM (0x10000000)
- Define `_stack_top` symbol at end of SRAM

### 📁 File Setup
```bash
cd ~/Desktop/vsdflow/task11
```

### Code - minimal.ld
```ld
ENTRY(_start)

MEMORY {
  FLASH (rx)  : ORIGIN = 0x00000000, LENGTH = 256K
  SRAM  (rwx) : ORIGIN = 0x10000000, LENGTH = 64K
}

SECTIONS {
  .text : {
    *(.text.start)
    *(.text)
    *(.rodata)
  } > FLASH

  .data : {
    _data_start = .;
    *(.data)
    _data_end = .;
  } > SRAM

  .bss : {
    _bss_start = .;
    *(.bss)
    _bss_end = .;
  } > SRAM

  _stack_top = ORIGIN(SRAM) + LENGTH(SRAM);
}
```

### Supporting Files

#### test_linker.c
```c
#include <stdint.h>
uint32_t global_var = 0x12345678;
uint32_t bss_var;
void test_function(void) {
    global_var = 0xABCDEF00;
    bss_var = 0x11111111;
}
void main(void) {
    test_function();
    while (1) {}
}
```

#### start.s
```asm
.section .text.start
.global _start
_start:
  lui sp, %hi(_stack_top)
  addi sp, sp, %lo(_stack_top)
  call main
1: j 1b
.size _start, . - _start
```

### 💻 Build Commands
```bash
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-gcc -c start.s -o start.o
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-gcc -c test_linker.c -o test_linker.o
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-ld -T minimal.ld start.o test_linker.o -o test_linker.elf
```

### 💻 Inspect ELF Output
```bash
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-objdump -h test_linker.elf
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-nm test_linker.elf
```

### 📋 Explanation
- .text should start at 0x00000000 (Flash)
- .data and .bss go into SRAM
- _start entry at 0x00000000
- ELF confirms correct memory placement

### 🖼️ Output Screenshot
📸 Screenshot of objdump -h and nm outputs

---

## Task 12: LED Blinking with Custom Linker

### 🎯 Objective
Blink an LED using memory-mapped I/O and compile it with a custom linker script.

### 📁 File Setup
```bash
cd ~/Desktop/vsdflow && rm -rf task12 && mkdir task12 && cd task12
```

### 💻 Code - task12_led_blink.c
```c
#include <stdint.h>
#define GPIO_BASE 0x10012000
#define GPIO_OUTPUT_REG (*(volatile uint32_t *)(GPIO_BASE + 0x00))
#define GPIO_DIRECTION_REG (*(volatile uint32_t *)(GPIO_BASE + 0x04))

void delay(volatile int count) {
    while(count--) { asm volatile("nop"); }
}

int main(void) {
    GPIO_DIRECTION_REG |= 0x1;
    while (1) {
        GPIO_OUTPUT_REG ^= 0x1;
        delay(100000);
    }
    return 0;
}
```

### led_start.s
```asm
.section .text.start
.global _start
_start:
    lui sp, %hi(_stack_top)
    addi sp, sp, %lo(_stack_top)
    call main
1:  j 1b
```

### led_blink.ld
```ld
ENTRY(_start)

MEMORY {
  FLASH (rx) : ORIGIN = 0x00000000, LENGTH = 256K
  SRAM  (rwx): ORIGIN = 0x10000000, LENGTH = 64K
}

SECTIONS {
  .text : {
    *(.text.start)
    *(.text)
    *(.rodata*)
  } > FLASH

  .data : {
    _data_start = .;
    *(.data)
    _data_end = .;
  } > SRAM

  .bss : {
    _bss_start = .;
    *(.bss)
    _bss_end = .;
  } > SRAM

  _stack_top = ORIGIN(SRAM) + LENGTH(SRAM);
}
```

### 💻 Build Script - build_led_blink.sh
```bash
#!/bin/bash
RISCV=/home/amritha-s/vsdflow/riscv/opt/riscv/bin

echo "=== Building LED Blink ==="
$RISCV/riscv32-unknown-elf-gcc -c led_start.s -o led_start.o
$RISCV/riscv32-unknown-elf-gcc -c task12_led_blink.c -o task12_led_blink.o
$RISCV/riscv32-unknown-elf-ld -T led_blink.ld led_start.o task12_led_blink.o -o task12_led_blink.elf

echo "=== ELF Sections ==="
$RISCV/riscv32-unknown-elf-objdump -h task12_led_blink.elf

echo "=== Disassembly ==="
$RISCV/riscv32-unknown-elf-objdump -d task12_led_blink.elf | less
```

### Execute Build
```bash
chmod +x build_led_blink.sh
./build_led_blink.sh
```

### 📋 Explanation
- GPIO is toggled using memory-mapped I/O
- Delay uses software nop loop
- Custom linker places code and variables properly
- Objdump output confirms working ELF

### 🖼️ Output Screenshot
📸 Screenshot of blinking logic disassembly or ELF sections

---

## Task 13: Interrupt Handling (MTIP)

### 🎯 Objective
Enable the machine-timer interrupt (MTIP) and write a simple handler in C/assembly.

### 🛠️ Files & Implementation

#### timer_interrupt.c
```c
#define UART_TX 0x10000000
#define UART_READY 0x10000005
#define MTIME 0x0200BFF8
#define MTIMECMP 0x02004000
typedef unsigned int uint32_t;
typedef unsigned long long uint64_t;

void uart_putc(char c) {
    volatile char* tx = (volatile char*)UART_TX;
    volatile char* rd = (volatile char*)UART_READY;
    while (!(*rd & (1 << 5)));
    *tx = c;
}

void uart_puts(const char* s) {
    while (*s) uart_putc(*s++);
}

void timer_handler(void) {
    uart_puts("MTIP\n");
    volatile uint64_t* mtime = (volatile uint64_t*)MTIME;
    volatile uint64_t* mtimecmp = (volatile uint64_t*)MTIMECMP;
    *mtimecmp = *mtime + 1000000;
}

void enable_timer_interrupt(void) {
    volatile uint64_t* mtime = (volatile uint64_t*)MTIME;
    volatile uint64_t* mtimecmp = (volatile uint64_t*)MTIMECMP;
    *mtimecmp = *mtime + 1000000;
    asm volatile("li t0, 0x80");
    asm volatile("csrs mie, t0");
    asm volatile("csrs mstatus, 0x8");
}

int main() {
    uart_putc('A');
    enable_timer_interrupt();
    uart_puts("Timer enabled\n");
    while (1) {
        uart_putc('.');
        for (volatile int i = 0; i < 100000; i++);
    }
    return 0;
}
```

#### trap_handler.s
```asm
.section .text
.global trap_handler
.align 4
trap_handler:
    addi sp, sp, -64
    sw ra, 0(sp)
    sw t0, 4(sp)
    sw t1, 8(sp)
    csrr t0, mcause
    li t1, 0x80000007       # Check for MTIP interrupt
    bne t0, t1, skip
    jal timer_handler
skip:
    lw ra, 0(sp)
    lw t0, 4(sp)
    lw t1, 8(sp)
    addi sp, sp, 64
    mret
```
startup13.s
asm
Copy
Edit
.section .text.start
.global _start
_start:
    la sp, _stack_top

    # Wait for UART to be ready
    li t0, 0x10000005       # UART_READY
    li t1, 0x20             # TX ready bit
wait_uart:
    lb t2, 0(t0)
    and t2, t2, t1
    beq t2, zero, wait_uart

    # Send initial 'S' character
    li t0, 0x10000000       # UART_TX
    li t1, 'S'
    sb t1, 0(t0)

    # Set trap vector and jump to main
    la t0, trap_handler
    csrw mtvec, t0
    jal main
1:  j 1b

.section .bss
.align 4
.space 1024
_stack_top:
linker13.ld
ld
Copy
Edit
OUTPUT_ARCH(riscv)
ENTRY(_start)
MEMORY
{
  FLASH (rx) : ORIGIN = 0x80000000, LENGTH = 16M
  RAM   (rw) : ORIGIN = 0x81000000, LENGTH = 16M
}
SECTIONS
{
  .text : { *(.text.start) *(.text) *(.rodata) } > FLASH
  .data : { *(.data) } > RAM AT > FLASH
  .bss  : { *(.bss) } > RAM
  _end = .;
}
🔧 Build & Run Commands:
bash
Copy
Edit
riscv32-unknown-elf-gcc -g -O0 \
  -march=rv32im -mabi=ilp32 \
  -nostdlib -T linker13.ld \
  pulse_interrupt.c trap_handler.s startup13.s \
  -o timer.elf

qemu-system-riscv32 -nographic -machine virt -bios none \
  -kernel timer.elf
🖼️ Output
nginx
Copy
Edit
SATimer enabled
........MTIP
MTIP
MTIP
MTIP
📋 Explanation & Issues
⭐ Used mie and mstatus CSR writes to enable MTIP.

✅ Interrupts triggered every approx 1,000,000 cycles.

🛠️ Fixes:

Initial missing CSR writes prevented MTIP.

Interrupt handler adjusted stack correctly and returned using mret.

Status: Completed.

📸 Screenshot Placeholder: Output showing periodic “MTIP” prints.

Task 14: RV32IMAC vs RV32IMC Comparison
🎯 Objective
Explain differences between RV32IMC and RV32IMAC, focusing on the atomic (A) extension.

📊 Comparison Table
Feature	RV32IMC (Base)	RV32IMAC (with A)
Integer (I)	✅	✅
Multiply/Divide (M)	✅	✅
Compressed (C)	✅	✅
Atomic (A)	❌	✅

🧍 Why “A” Matters
Adds atomic instructions: lr.w, sc.w, amoadd.w, amoswap.w, amoand.w, amoor.w, amomin.w, amomax.w.

Enables safe lock-free and multi-core synchronization.

📸 Screenshot Placeholder: Slide or diagram comparing ISA variants.

Task 15: Atomic Operations Test
🎯 Objective
Implement a mutex using lr.w/sc.w to safely increment a shared counter from two “threads” in bare-metal C.

📁 task15.c
(Uses memory mapped UART and atomic instructions)

c
Copy
Edit
#define UART_TX 0x10000000
#define UART_READY 0x10000005

typedef unsigned int uint32_t;
volatile uint32_t shared_counter = 0;
volatile uint32_t mutex = 0;

void uart_putc(char c) { /* ... */ }
void uart_puts(const char* s) { /* ... */ }
int mutex_lock(volatile uint32_t* m) { /* lr.w/sc.w loop */ }
void mutex_unlock(volatile uint32_t* m) { *m = 0; }

void thread1(void) { /* lock, increment, UART print */ }
void thread2(void) { /* same */ }

int main() {
  uart_puts("Starting threads\n");
  thread1(); thread2(); thread1(); thread2();
  uart_puts("Done\n");
  while(1) uart_putc('.'); // spinning
}
🔗 linker15.ld
ld
Copy
Edit
OUTPUT_ARCH(riscv)
ENTRY(_start)
MEMORY {
  FLASH (rx) : ORIGIN = 0x80000000, LENGTH = 16M
  RAM   (rw) : ORIGIN = 0x81000000, LENGTH = 16M
}
SECTIONS {
  .text : { *(.text.start) *(.text) } > FLASH
  .rodata { *(.rodata) } > FLASH
  .data { *(.data) } > RAM AT > FLASH
  .bss  { *(.bss) } > RAM
  _end = .;
}
🔧 Build & Run:
bash
Copy
Edit
riscv32-unknown-elf-gcc -g -O0 -march=rv32imac -mabi=ilp32 \
  -nostdlib -T linker15.ld \
  task15.c Startup15.s \
  -o task15.elf

qemu-system-riscv32 -nographic -machine virt -bios none \
  -kernel task15.elf
🖼️ Output
vbnet
Copy
Edit
Starting threads
T1: Enter critical section
T1: Counter = 1
T1: Exit critical section
T2: Enter critical section
T2: Counter = 2
...
Done
........
✅ Status: Completed mutex test with safe increments

📸 Screenshot Placeholder: UART output showing thread interleaving.

Task 16: Newlib printf() on Bare‑Metal
🎯 Objective
Use Newlib's printf() on bare-metal by implementing low-level syscalls (_write, _sbrk, etc.) redirecting output to UART.

📁 task16.c
(Contains UART helper, syscall stubs, main with printf)

linker16.ld & startup16.s
(Similar to Task 15 & 17—the FLASH/RAM layout)

🔧 Build & Run:
bash
Copy
Edit
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -c startup16.s task16.c
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 \
  -nostartfiles -T linker16.ld startup16.o task16.o \
  -lc -lgcc -o task16.elf

qemu-system-riscv32 -nographic -machine virt -bios none \
  -kernel task16.elf
🖼️ Output
yaml
Copy
Edit
SAHello, RISC‑V! Counter: 42
........
✅ Issues resolved:

Needed _fstat, _isatty stubs for printf to work.

_sbrk bound-checked to prevent heap overflow.

📸 Screenshot Placeholder: Terminal showi ng printf output and blinking delays.

Task 17: Endianness & Struct Packing
🎯 Objective
Use a union-based C test to confirm that RISC-V is little-endian.

📁 task17.c
(Prints test value and individual bytes over UART)

linker17.ld & startup17.s
(Custom FLASH/RAM layout and basic start sequence)

🔧 Build & Run:
bash
Copy
Edit
riscv32-unknown-elf-gcc -c task17.c startup17.s \
  -march=rv32imac -mabi=ilp32 -Os

riscv32-unknown-elf-gcc task17.o startup17.o \
  -march=rv32imac -mabi=ilp32 \
  -nostdlib -T linker17.ld -lc -lgcc \
  -o task17.elf

qemu-system-riscv32 -M virt -bios none -kernel task17.elf -nographic
🖼️ Output
yaml
Copy
Edit
Bare‑metal RISC‑V Application
Value of x: 43
Verifying Byte Ordering (Endianness):
Value stored: 0x01020304
Bytes in memory:
Byte 0: 0x04
Byte 1: 0x03
Byte 2: 0x02
Byte 3: 0x01

This system is Little‑Endian.
...
✅ Fixes:

Added newline printing for readability.

volatile to prevent compiler optimizations disrupting union test.

📸 Screenshot Placeholder: Terminal logs confirming endianness.

