# ARM Assembly Programming for Embedded Systems
## 1. ✅ **Basic Instructions from ARM ISA**

### ARM supports **three main instruction sets**:

  * **A32 (ARM mode)**: 32-bit fixed-length instructions (legacy).
  * **T32 (Thumb mode)**: Mixed 16/32-bit instructions (commonly used in Cortex-M).
  * **A64**: 64-bit ARMv8 architecture.
* Common basic instructions (Thumb/ARM):

  * `MOV` — move data between registers.
  * `ADD`, `SUB` — arithmetic.
  * `LDR`, `STR` — load/store from/to memory.
  * `B`, `BL` — branch and branch with link (function call).
  * `CMP`, `BEQ`, `BNE` — compare and conditional branch.
  * `PUSH`, `POP` — stack manipulation.
* Example:

  ```asm
  MOV R0, #10
  ADD R1, R0, #5
  ```

---

### 2. ✅ **Writing an Assembly File and Assembling**

* Write code in a `.s` file (ARM Assembly):

  ```asm
  .global _start
  _start:
      MOV R0, #1
      ADD R1, R0, #2
      B .
  ```
* Assemble and convert to object code using **GNU assembler (as)**:

  ```bash
  arm-none-eabi-as -mcpu=cortex-m4 -o program.o program.s
  ```
* Link to machine code (binary):

  ```bash
  arm-none-eabi-ld -T linker.ld -o program.elf program.o
  arm-none-eabi-objcopy -O binary program.elf program.bin
  ```

---

### 3. ✅ **How CPU Starts Executing First Instruction (ARM Boot Sequence)**

* On **reset**, ARM Cortex-M CPUs:

  * Load **initial Stack Pointer (SP)** from address `0x00000000`.
  * Load **Reset Handler address (PC)** from `0x00000004`.
  * Jump to Reset Handler, typically `_start` or `Reset_Handler`.
* **Vector Table** at `0x00000000` (startup.s):

  ```asm
  .section .vectors
  .word _stack_top
  .word Reset_Handler
  ```
* Boot ROM or Flash maps code starting from `0x00000000`.

---

### 4. ✅ **Linker Scripts and Code Placement**

* Linker script (`linker.ld`) controls memory layout:

  ```ld
  MEMORY
  {
    FLASH (rx) : ORIGIN = 0x08000000, LENGTH = 512K
    RAM (rwx) : ORIGIN = 0x20000000, LENGTH = 128K
  }

  SECTIONS
  {
    .text : {
        *(.vectors)
        *(.text*)
    } > FLASH

    .data : {
        *(.data*)
    } > RAM AT > FLASH

    .bss : {
        *(.bss*)
    } > RAM
  }
  ```
* Use linker to place code precisely:

  ```bash
  arm-none-eabi-ld -T linker.ld -o output.elf program.o
  ```
* This ensures:

  * Code (`.text`) is placed in **Flash**.
  * Data (`.data`, `.bss`) in **RAM**.
  * Startup begins at `0x08000000` (or `0x00000000` remapped).


## ✅ 1. Programmer’s Model (Cortex-A)

### 📌 Key Points:

* **General Purpose Registers**: R0-R12
* **Special Registers**:

  * R13 (SP), R14 (LR), R15 (PC)
  * **Program Status Registers**: CPSR (Current), SPSR (Saved Program Status for exceptions)
* **Multiple Processor Modes**: User, FIQ, IRQ, Supervisor, Abort, Undefined, System, Monitor (TrustZone)
* Banked registers for exception modes (e.g., separate R13, R14).

### 🎤 Interview Trick:

**Q:** Why does ARM Cortex-A use banked registers in privileged modes?
**A:** Banked registers enable **faster context switching** during exceptions (e.g., IRQ, FIQ) without corrupting user-mode context.

---

## ✅ 2. Instruction Set Architecture (ISA)

### 📌 Key Points:

* Supports **A32** (32-bit ARM), **T32** (Thumb-2), and **A64** (64-bit ARMv8-A).
* Cortex-A often supports both ARM (A32) and Thumb (T32); ARMv8-A adds A64.
* **Load-Store architecture**, heavy pipelining, SIMD (NEON), FP support.
* Conditional execution is limited in A64 (most removed except CBZ/CBNZ).

### 🎤 Interview Trick:

**Q:** Difference between Cortex-A Thumb mode vs Cortex-M Thumb mode?
**A:** Cortex-A Thumb (T32) supports mixed 16/32-bit instructions for better **code density**, while Cortex-M uses Thumb exclusively with less complex privilege levels.

---

## ✅ 3. Exception Model (Cortex-A)

### 📌 Key Points:

* Supports synchronous exceptions (Reset, Undefined Instruction, Supervisor Call (SVC), Prefetch Abort, Data Abort) and asynchronous exceptions (IRQ, FIQ).
* **Vector Table** typically starts at 0x00000000 or 0xFFFF0000 (configurable).
* **FIQ** is highest priority with dedicated banked registers (R8-R14).
* Exception Level (EL) concept in ARMv8-A: EL0 (user), EL1 (kernel), EL2 (hypervisor), EL3 (secure monitor).

### 🎤 Interview Trick:

**Q:** Why does Cortex-A have FIQ and IRQ?
**A:** **FIQ** has higher priority and **lower interrupt latency** due to more banked registers, suitable for critical fast-response tasks.

---

## ✅ 4. Memory Model (Cortex-A)

### 📌 Key Points:

* **Memory Management Unit (MMU)** for virtual memory → used in Linux systems.
* Supports **caches (L1, L2)**, **cache coherency**, **write buffers**.
* Implements **Barrier Instructions**:

  * `DSB` (Data Synchronization Barrier),
  * `DMB` (Data Memory Barrier),
  * `ISB` (Instruction Synchronization Barrier).
* Rich memory types: Device, Normal, Strongly Ordered regions.
* TrustZone (TZASC) divides secure/non-secure memory.

### 🎤 Interview Trick:

**Q:** Difference between DMB and DSB?
**A:** **DSB** completes all memory accesses before continuing execution; **DMB** only orders memory operations without stalling execution globally.

---

## ✅ 5. Debug Model (Cortex-A)

### 📌 Key Points:

* **CoreSight Debug Architecture**:

  * **JTAG/SWD**, ETM (Embedded Trace Macrocell), PTM (Program Trace Macrocell),
  * **Debug registers** accessible via coprocessor instructions (CP15).
* Hardware breakpoints, watchpoints, trace functionality.
* Linux uses `perf`, `ftrace`, `gdb` with hardware support.

### 🎤 Interview Trick:

**Q:** How is debugging on Cortex-A more advanced than Cortex-M?
**A:** Cortex-A supports **full MMU, virtual memory-aware debugging**, rich trace features (ETM/PTM), and **hardware-assisted profiling**, unlike Cortex-M's simpler debug model.

---

## ✅ Summary — Cortex-A Interview Focus

| Topic            | Cortex-A Focus                                 |
| ---------------- | ---------------------------------------------- |
| Programmer Model | Privileged modes, banked registers, SPSR       |
| ISA              | ARM, Thumb, A64, SIMD (NEON), FP               |
| Exception Model  | Multi-level exceptions, EL0-EL3 in ARMv8       |
| Memory Model     | MMU, caching, barriers, TrustZone              |
| Debug Model      | CoreSight, ETM/PTM, virtual memory-aware tools |

