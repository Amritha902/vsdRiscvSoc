# RISC-V Toolchain Tasks - Amritha S - Week 1



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
### Screenshot 
![image](https://github.com/user-attachments/assets/3dcee291-f9e9-4493-9350-c8e908410656)

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
### Screenshot
![image](https://github.com/user-attachments/assets/af39c9da-4a96-4403-bc80-46136fe9ab16)
![image](https://github.com/user-attachments/assets/b71a577c-bd4f-4cba-963d-2aa157d94936)
![image](https://github.com/user-attachments/assets/e5ae8391-2e7e-4cd0-a475-78df5161ac00)


### 📋 Task Checklist
| Task # | Description | Done? |
|--------|-------------|-------|
| 2.1 | Copy or reuse hello.c from Task 1 | ✅ |
| 2.2 | Compile to object file: hello.o | ✅ |
| 2.3 | Link to ELF executable: hello.elf | ✅ |
| 2.4 | Confirm ELF file is created via ls -lh hello.elf | ✅ |
| 2.5 | Capture screenshot of ELF generation in terminal | 🔲 |



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
![image](https://github.com/user-attachments/assets/3dfb9a0c-2cfc-4124-a621-027dfce0abf3)
![image](https://github.com/user-attachments/assets/ac0793a8-2c29-4776-80fe-fad648dda65b)

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
# 🧠 Task 4: Compile and Run Custom C Code on RISC-V

## ❓ Objective
Write a simple C program (`hello.c`) and compile it using the RISC-V toolchain. Run the resulting binary using QEMU.

## 📁 Project Structure
```
task4/
├── README.md
└── hello.c
```

## 📄 Source Code

**File:** `hello.c`
```c
int main() {
    while (1);
    return 0;
}
```

## 🧱 Compilation

Compile the C program using the RISC-V GCC toolchain:

```bash
/home/amritha-s/vsdflow/riscv/opt/riscv/bin/riscv32-unknown-elf-gcc \
  -o hello.elf hello.c
```

## ▶️ Screenshot:

![image](https://github.com/user-attachments/assets/b353269b-0dfb-4e74-9f6f-5746e8079300)
![image](https://github.com/user-attachments/assets/a9cfa8a6-b259-46a3-aad6-8c3bbb0b53d6)



## 📌 Notes

- This uses a bare minimum infinite loop program
- Since there's no output or UART involved, nothing prints to the terminal
- Used QEMU's user-mode emulator (`qemu-riscv32`) to validate ELF format
- The program demonstrates successful cross-compilation for RISC-V architecture

## ✅ Status
**Completed** - Successfully compiled C code for RISC-V and executed using QEMU emulation.

---

### 🔧 Requirements
- RISC-V GCC toolchain
- QEMU with RISC-V support (`qemu-riscv32`)

### 🎯 Learning Outcomes
- Cross-compilation for RISC-V architecture
- Understanding ELF binary format
- QEMU user-mode emulation basics



  
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
![image](https://github.com/user-attachments/assets/4a6d8054-7684-41ea-ba9c-e623eb2ec5e7)


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
![image](https://github.com/user-attachments/assets/637eaef2-13cd-4286-a28a-0da038b5bb80)
![image](https://github.com/user-attachments/assets/fba3523e-bc19-4737-a849-95d83b95668a)


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
![image](https://github.com/user-attachments/assets/f8278ea2-af4d-45df-83c2-14adce8b9ae8)

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
![image](https://github.com/user-attachments/assets/9b9e249c-04eb-47d8-b899-ef77d50d8aa6)

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
![image](https://github.com/user-attachments/assets/d6474602-701c-439d-9df1-860ef3fb5183)
![image](https://github.com/user-attachments/assets/5f01ab15-73f1-4ab3-a46e-dcfe4e4e8bca)

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
![image](https://github.com/user-attachments/assets/c702600e-1e61-4252-a2de-cc148e1c28ff)


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
![image](https://github.com/user-attachments/assets/4adce780-0e86-4e28-83ed-6096f5259183)
![image](https://github.com/user-attachments/assets/5e0d97eb-a246-4295-b63f-7a2779bbe4e5)
![image](https://github.com/user-attachments/assets/31d9cdca-4be4-49a0-91e6-9b3d169d2d45)

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
![image](https://github.com/user-attachments/assets/6c6835ed-c6b9-457d-be69-19a1c18cfbb1)
![image](https://github.com/user-attachments/assets/86dfeca6-f138-44c4-9a54-208aa6be0663)

---


## Task 13: Interrupt Primer (MTIP Handler)

### 🎯 Objective
Enable the machine-timer interrupt (MTIP) and write a simple handler in C/assembly to demonstrate interrupt handling in RISC-V bare-metal programming.

### 📁 File Structure
```
~/Desktop/vsdflow/task13/
├── timer_interrupt.c
├── trap_handler.s
├── startup13.s
├── linker13.ld
└── timer.elf
```

### 💻 Code Files

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
    *mtimecmp = *mtime + 1000000;  // Reset timer for next interrupt
}

void enable_timer_interrupt(void) {
    volatile uint64_t* mtime = (volatile uint64_t*)MTIME;
    volatile uint64_t* mtimecmp = (volatile uint64_t*)MTIMECMP;
    *mtimecmp = *mtime + 1000000;  // Set initial timer
    
    // Enable machine timer interrupt (bit 7 in MIE)
    asm volatile("li t0, 0x80");
    asm volatile("csrs mie, t0");
    
    // Enable global interrupts (bit 3 in MSTATUS)
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
```assembly
.section .text
.global trap_handler
.align 4

trap_handler:
    # Save context
    addi sp, sp, -64
    sw ra, 0(sp)
    sw t0, 4(sp)
    sw t1, 8(sp)
    
    # Check if it's a timer interrupt (mcause = 0x80000007)
    csrr t0, mcause
    li t1, 0x80000007
    bne t0, t1, skip
    
    # Call timer handler
    jal timer_handler
    
skip:
    # Restore context
    lw ra, 0(sp)
    lw t0, 4(sp)
    lw t1, 8(sp)
    addi sp, sp, 64
    
    # Return from trap
    mret
```

#### startup13.s
```assembly
.section .text.start
.global _start

_start:
    # Initialize stack pointer
    la sp, _stack_top
    
    # Wait for UART ready
    li t0, 0x10000005
    li t1, 0x20
wait_uart:
    lb t2, 0(t0)
    and t2, t2, t1
    beq t2, zero, wait_uart
    
    # Send startup character
    li t0, 0x10000000
    li t1, 'S'
    sb t1, 0(t0)
    
    # Set trap vector
    la t0, trap_handler
    csrw mtvec, t0
    
    # Jump to main
    jal main
    
    # Infinite loop if main returns
    j .

.section .bss
.align 4
.space 1024
_stack_top:
```

#### linker13.ld
```ld
OUTPUT_ARCH(riscv)
ENTRY(_start)

MEMORY {
    FLASH (rx) : ORIGIN = 0x80000000, LENGTH = 16M
    RAM   (rw) : ORIGIN = 0x81000000, LENGTH = 16M
}

SECTIONS {
    .text : {
        *(.text.start)
        *(.text*)
    } > FLASH
    
    .rodata : ALIGN(4) {
        *(.rodata*)
    } > FLASH
    
    .data : ALIGN(4) {
        *(.data*)
    } > RAM AT > FLASH
    
    .bss : ALIGN(4) {
        *(.bss*)
    } > RAM
    
    _end = .;
}
```

### 🔨 Build Commands
```bash
cd ~/Desktop/vsdflow/task13

# Compile and link
riscv32-unknown-elf-gcc -g -O0 -march=rv32im -mabi=ilp32 -nostdlib \
  -T linker13.ld \
  -o timer.elf \
  timer_interrupt.c startup13.s trap_handler.s

# Run in QEMU
qemu-system-riscv32 -nographic -machine virt -bios none -kernel timer.elf
```

### 📋 Expected Output
```
SATimer enabled
........MTIP
MTIP
MTIP
MTIP
```

### 📘 Explanation
This task demonstrates:
- Setting up machine timer interrupts (MTIP)
- Writing interrupt handlers in assembly
- Context saving/restoration in trap handlers
- CSR manipulation for interrupt control
- Memory-mapped UART communication

---

## Task 14: RV32IMAC vs RV32IMC ("A" Extension)

### 🎯 Objective
Compare RV32IMC vs RV32IMAC instruction sets, focusing on atomic instruction capabilities.

### 📊 Feature Comparison

| Feature | RV32IMC | RV32IMAC | Description |
|---------|---------|----------|-------------|
| I (Base Integer) | ✅ | ✅ | Basic integer operations |
| M (Multiply/Divide) | ✅ | ✅ | Hardware multiply/divide |
| C (Compressed) | ✅ | ✅ | 16-bit compressed instructions |
| A (Atomic) | ❌ | ✅ | Atomic memory operations |

### 🔸 Atomic Instruction Set

| Instruction | Meaning | Use Case |
|-------------|---------|----------|
| `lr.w` | Load-Reserved | Begin read-modify-write atomic operation |
| `sc.w` | Store-Conditional | Complete atomic store if reservation valid |
| `amoadd.w` | Atomic Add | Atomic increment of memory location |
| `amoswap.w` | Atomic Swap | Exchange value atomically |
| `amoor.w` | Atomic OR | Set flags atomically |
| `amoand.w` | Atomic AND | Clear flags atomically |
| `amomin.w` | Atomic Min | Resource arbitration |
| `amomax.w` | Atomic Max | Tracking maximum values safely |

### 📘 Explanation
The "A" extension provides:
- **Lock-free programming**: Enable efficient concurrent algorithms
- **Synchronization primitives**: Build mutexes, semaphores, barriers
- **Atomic updates**: Safe counter increments, flag operations
- **Memory ordering**: Acquire/release semantics for multi-core systems

---

## Task 15: Atomic Test Program (lr.w/sc.w Mutex)

### 🎯 Objective
Implement a spin-lock mutex using `lr.w`/`sc.w` atomic instructions to simulate thread synchronization.

### 📁 File Structure
```
~/Desktop/vsdflow/task15/
├── task15.c
├── startup15.s
├── linker15.ld
└── task15.elf
```

### 💻 Code Files

#### task15.c
```c
#include <stdint.h>

#define UART_TX 0x10000000
#define UART_READY 0x10000005

// Global variables
volatile uint32_t mutex = 0;     // 0 = unlocked, 1 = locked
volatile uint32_t counter = 0;
volatile uint32_t thread_id = 1;

void uart_putc(char c) {
    volatile char* tx = (volatile char*)UART_TX;
    volatile char* rd = (volatile char*)UART_READY;
    while (!(*rd & (1 << 5)));
    *tx = c;
}

void uart_puts(const char* s) {
    while (*s) uart_putc(*s++);
}

// Atomic mutex lock using lr.w/sc.w
int mutex_lock(volatile uint32_t* lock) {
    uint32_t result;
    do {
        asm volatile (
            "lr.w %0, (%1)\n"      // Load-reserved from lock address
            "bnez %0, 1f\n"        // If already locked, retry
            "li %0, 1\n"           // Set lock value to 1
            "sc.w %0, %0, (%1)\n"  // Store-conditional
            "1:"
            : "=&r" (result)
            : "r" (lock)
            : "memory"
        );
    } while (result != 0);  // Retry if sc.w failed
    return 0;
}

// Atomic mutex unlock
void mutex_unlock(volatile uint32_t* lock) {
    asm volatile (
        "amoswap.w zero, zero, (%0)"
        :
        : "r" (lock)
        : "memory"
    );
}

void thread_function(uint32_t id) {
    char msg[32];
    
    // Simple sprintf equivalent
    msg[0] = 'T';
    msg[1] = '0' + id;
    msg[2] = ':';
    msg[3] = ' ';
    msg[4] = '\0';
    
    for (int i = 0; i < 5; i++) {
        mutex_lock(&mutex);
        
        // Critical section
        uart_puts(msg);
        uart_puts("Enter critical section\n");
        
        counter++;
        
        uart_puts(msg);
        uart_puts("Counter = ");
        uart_putc('0' + counter);
        uart_putc('\n');
        
        // Simulate work
        for (volatile int j = 0; j < 10000; j++);
        
        uart_puts(msg);
        uart_puts("Exit critical section\n");
        
        mutex_unlock(&mutex);
        
        // Non-critical section
        for (volatile int j = 0; j < 50000; j++);
    }
}

int main() {
    uart_putc('S');
    uart_putc('A');
    uart_puts("Starting threads\n");
    
    // Simulate two threads
    thread_function(1);
    thread_function(2);
    
    uart_puts("Done\n");
    
    while (1) {
        uart_putc('.');
        for (volatile int i = 0; i < 100000; i++);
    }
    
    return 0;
}
```

#### startup15.s
```assembly
.section .text.start
.global _start

_start:
    # Initialize stack
    la sp, _stack_top
    
    # Wait for UART ready
    li t0, 0x10000005
    li t1, 0x20
wait_uart:
    lb t2, 0(t0)
    and t2, t2, t1
    beq t2, zero, wait_uart
    
    # Jump to main
    jal main
    
    # Infinite loop if main returns
    j .

.section .bss
.align 4
.space 2048
_stack_top:
```

#### linker15.ld
```ld
OUTPUT_ARCH(riscv)
ENTRY(_start)

MEMORY {
    FLASH (rx) : ORIGIN = 0x80000000, LENGTH = 16M
    RAM   (rw) : ORIGIN = 0x81000000, LENGTH = 16M
}

SECTIONS {
    .text : {
        *(.text.start)
        *(.text*)
    } > FLASH
    
    .rodata : ALIGN(4) {
        *(.rodata*)
    } > FLASH
    
    .data : ALIGN(4) {
        *(.data*)
    } > RAM AT > FLASH
    
    .bss : ALIGN(4) {
        *(.bss*)
    } > RAM
    
    _end = .;
}
```

### 🔨 Build Commands
```bash
cd ~/Desktop/vsdflow/task15

# Compile with atomic extension
riscv32-unknown-elf-gcc -g -O0 -march=rv32imac -mabi=ilp32 -nostdlib \
  -T linker15.ld \
  -o task15.elf \
  task15.c startup15.s

# Run in QEMU
qemu-system-riscv32 -nographic -machine virt -bios none -kernel task15.elf
```

### 📋 Expected Output
```
SAStarting threads
T1: Enter critical section
T1: Counter = 1
T1: Exit critical section
T2: Enter critical section
T2: Counter = 2
T2: Exit critical section
...
Done
........
```

### 📘 Explanation
This task demonstrates:
- **Atomic operations**: Using `lr.w`/`sc.w` for lock-free synchronization
- **Mutex implementation**: Spin-lock using atomic instructions
- **Critical sections**: Protecting shared resources (counter)
- **Thread simulation**: Sequential execution simulating concurrent access

---

## Task 16: Newlib printf on Bare-Metal

### 🎯 Objective
Retarget Newlib's `printf()` function to work with memory-mapped UART in a bare-metal RISC-V environment.

### 📁 File Structure
```
~/Desktop/vsdflow/task16/
├── task16.c
├── startup16.s
├── linker16.ld
└── task16.elf
```

### 💻 Code Files

#### task16.c
```c
#include <stdio.h>
#include <errno.h>
#include <sys/stat.h>

#define UART_TX 0x10000000
#define UART_READY 0x10000005

// Heap management
extern char _end;
static char* heap_ptr = &_end;
static char* heap_limit = (char*)0x82000000;  // 16MB limit

// UART functions
void uart_putc(char c) {
    volatile char* tx = (volatile char*)UART_TX;
    volatile char* rd = (volatile char*)UART_READY;
    while (!(*rd & (1 << 5)));
    *tx = c;
}

// Newlib system call stubs
int _write(int file, char* ptr, int len) {
    if (file == 1 || file == 2) {  // stdout or stderr
        for (int i = 0; i < len; i++) {
            uart_putc(ptr[i]);
        }
        return len;
    }
    errno = EBADF;
    return -1;
}

int _read(int file, char* ptr, int len) {
    (void)file;
    (void)ptr;
    (void)len;
    errno = ENOSYS;
    return -1;
}

int _close(int file) {
    (void)file;
    return -1;
}

int _lseek(int file, int offset, int whence) {
    (void)file;
    (void)offset;
    (void)whence;
    return -1;
}

int _fstat(int file, struct stat* st) {
    if (file >= 0 && file <= 2) {  // stdin, stdout, stderr
        st->st_mode = S_IFCHR;
        return 0;
    }
    errno = EBADF;
    return -1;
}

int _isatty(int file) {
    if (file >= 0 && file <= 2) {
        return 1;  // stdin, stdout, stderr are TTY
    }
    return 0;
}

void* _sbrk(int incr) {
    char* prev_heap = heap_ptr;
    
    if (heap_ptr + incr > heap_limit) {
        errno = ENOMEM;
        return (void*)-1;
    }
    
    heap_ptr += incr;
    return prev_heap;
}

void _exit(int status) {
    (void)status;
    while (1) {
        // Infinite loop
    }
}

int _kill(int pid, int sig) {
    (void)pid;
    (void)sig;
    errno = ENOSYS;
    return -1;
}

int _getpid(void) {
    return 1;
}

// Application code
int main() {
    uart_putc('S');
    uart_putc('A');
    
    // Test printf functionality
    printf("Hello, RISC-V! Counter: %d\n", 42);
    printf("Hex value: 0x%08X\n", 0xDEADBEEF);
    printf("Float test: %.2f\n", 3.14159);
    
    int counter = 0;
    while (1) {
        printf("Loop iteration: %d\n", counter++);
        
        // Delay
        for (volatile int i = 0; i < 500000; i++);
        
        if (counter >= 5) break;
    }
    
    printf("Program completed successfully!\n");
    
    while (1) {
        uart_putc('.');
        for (volatile int i = 0; i < 100000; i++);
    }
    
    return 0;
}
```

#### startup16.s
```assembly
.section .text.start
.global _start

_start:
    # Initialize stack
    la sp, _stack_top
    
    # Clear BSS section
    la t0, _bss_start
    la t1, _bss_end
clear_bss:
    beq t0, t1, bss_done
    sw zero, 0(t0)
    addi t0, t0, 4
    j clear_bss
bss_done:
    
    # Wait for UART ready
    li t0, 0x10000005
    li t1, 0x20
wait_uart:
    lb t2, 0(t0)
    and t2, t2, t1
    beq t2, zero, wait_uart
    
    # Jump to main
    jal main
    
    # Infinite loop if main returns
    j .

.section .bss
.align 4
.space 4096
_stack_top:
```

#### linker16.ld
```ld
OUTPUT_ARCH(riscv)
ENTRY(_start)

MEMORY {
    FLASH (rx) : ORIGIN = 0x80000000, LENGTH = 16M
    RAM   (rw) : ORIGIN = 0x81000000, LENGTH = 16M
}

SECTIONS {
    .text : {
        *(.text.start)
        *(.text*)
    } > FLASH
    
    .rodata : ALIGN(4) {
        *(.rodata*)
    } > FLASH
    
    .data : ALIGN(4) {
        _data_start = .;
        *(.data*)
        _data_end = .;
    } > RAM AT > FLASH
    
    .bss : ALIGN(4) {
        _bss_start = .;
        *(.bss*)
        *(.sbss*)
        _bss_end = .;
    } > RAM
    
    _end = .;
}
```

### 🔨 Build Commands
```bash
cd ~/Desktop/vsdflow/task16

# Compile separately
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 \
  -c startup16.s task16.c

# Link with newlib
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -nostartfiles \
  -T linker16.ld \
  -o task16.elf startup16.o task16.o

# Run in QEMU
qemu-system-riscv32 -nographic -machine virt -bios none -kernel task16.elf
```

### 📋 Expected Output
```
SAHello, RISC-V! Counter: 42
Hex value: 0xDEADBEEF
Float test: 3.14
Loop iteration: 0
Loop iteration: 1
...
Program completed successfully!
........
```

### 📘 Explanation
This task demonstrates:
- **Newlib integration**: Retargeting standard C library for bare-metal
- **System call implementation**: `_write`, `_sbrk`, `_fstat`, etc.
- **Printf functionality**: Full formatted output support
- **Heap management**: Dynamic memory allocation support
- **BSS initialization**: Proper C runtime setup

---

# TASK 17 — Endianness & Struct Packing Check

## ✅ Objective
Verify RV32 endianness with union trick and print byte order via UART on bare-metal RISC-V system.

## 🛠️ Project Structure
```
task17/
├── task17.c          # Main application with endianness detection
├── startup17.s       # Assembly startup code
├── linker17.ld       # Linker script for memory layout
└── task17.elf        # Final executable
```

## 🔧 Build Process

### Prerequisites
- RISC-V GCC toolchain (`riscv32-unknown-elf-gcc`)
- QEMU RISC-V emulator (`qemu-system-riscv32`)

### Compilation Commands
```bash
# Compile C source
riscv32-unknown-elf-gcc -c task17.c -march=rv32imac -mabi=ilp32 -Os

# Compile assembly startup
riscv32-unknown-elf-gcc -c startup17.s -march=rv32imac -mabi=ilp32

# Link final executable
riscv32-unknown-elf-gcc startup17.o task17.o -march=rv32imac -mabi=ilp32 \
    -nostartfiles -T linker17.ld -o task17.elf
```

### Run in QEMU
```bash
qemu-system-riscv32 -nographic -machine virt -bios none -kernel task17.elf
```

## 💬 Expected Output
```
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
```

## 🔍 Technical Details

### Endianness Detection Method
- Uses a `union` containing both a 32-bit integer and a 4-byte array
- Stores the value `0x01020304` as an integer
- Reads back individual bytes to determine storage order
- Little-endian: least significant byte stored first (0x04, 0x03, 0x02, 0x01)
- Big-endian: most significant byte stored first (0x01, 0x02, 0x03, 0x04)

### Architecture Configuration
- **ISA**: RV32IMAC (32-bit base + Integer Multiply/Divide + Atomic + Compressed)
- **ABI**: ilp32 (Integer-Long-Pointer 32-bit)
- **Target**: Bare-metal (no OS, direct hardware access)

## 🐞 Issues Faced & Solutions

### Problem: Output Formatting
- **Issue**: Output appeared on single line without proper formatting
- **Solution**: Added explicit `\n` newline characters for readability

### Problem: Compiler Optimization
- **Issue**: Union might be optimized away, preventing endianness test
- **Solution**: Used `volatile` keyword to ensure memory access occurs as written

### Problem: UART Output
- **Issue**: Required proper UART initialization for console output
- **Solution**: Implemented memory-mapped UART writes to QEMU's virtual UART

## 📋 Key Learning Points
1. **Endianness Verification**: RISC-V uses little-endian byte ordering by default
2. **Union Usage**: Effective technique for low-level memory layout inspection
3. **Bare-Metal I/O**: Direct memory-mapped peripheral access without OS abstraction
4. **Compiler Considerations**: Volatile keywords necessary to prevent optimization interference

## 🎯 Success Criteria
- ✅ Successfully compiles with RV32IMAC ISA
- ✅ Runs on QEMU RISC-V virtual machine
- ✅ Correctly identifies little-endian byte ordering
- ✅ Outputs formatted results via UART console
- ✅ Demonstrates union-based memory inspection technique

---
*Task completed successfully - RISC-V endianness verification working as expected*
