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


## Interrupt

The interrupt is a signal emitted by hardware or software when a process or an event needs immediate attention. It alerts the processor to a high-priority process requiring interruption of the current working process. In I/O devices one of the bus control lines is dedicated for this purpose and is called the Interrupt Service Routine (ISR).

- interrupt latency: the delay between the time an interrupt is received and the start of the execution of the ISR.

![Interrupt](https://media.geeksforgeeks.org/wp-content/uploads/20241210153745065033/Types-of-Interrupt.png)


#### Sequence of Interrupt Handling
1. **Interrupt Request**: Hardware or software generates an interrupt request.

2. **Interrupt Acknowledgment**: The processor acknowledges the interrupt and saves the current context.

3. **Interrupt Vectoring**: The processor uses the interrupt vector table to find the appropriate ISR.

4. **ISR Execution**: The processor executes the ISR to handle the interrupt.

5. **Context Restoration**: After the ISR completes, the processor restores the previous context and resumes normal execution.

6. **End of Interrupt**: The processor signals the end of the interrupt, allowing further interrupts to be processed.

![Interrupt Handling Sequence](https://media.geeksforgeeks.org/wp-content/uploads/20240801184747/Flowchart.png)



#### Managing Multiple Interrupts

- polling : The CPU periodically checks the status of devices to see if they need attention. This can be inefficient and slow, especially if many devices are involved. the first device that needs attention(IRQ Bit is set) is serviced, and the rest are ignored until the next polling cycle.
- vectored interrupts : Each interrupt source has a unique vector number, allowing the CPU to quickly identify the ISR to execute. This reduces the overhead of searching for the ISR in a table. This unique vector number can be the starting address if the ISR or where the ISR is stored in a table(Interrupt Vector Table).
- Interrupt nesting : Allows higher-priority interrupts to preempt lower-priority ISRs. This ensures that critical tasks are handled promptly, but it requires careful management to avoid stack overflow and ensure proper context switching.


#### Interrupt Vector Table
The Interrupt Vector Table (IVT) is a data structure that maps interrupt requests to their corresponding Interrupt Service Routines (ISRs). It is typically located at a fixed address in memory, allowing the CPU to quickly access the appropriate ISR when an interrupt occurs.
The IVT contains entries for each interrupt source, with each entry pointing to the starting address of the corresponding ISR. When an interrupt occurs, the CPU uses the interrupt vector number to index into the IVT and retrieve the address of the ISR to execute.



#### Interrupt Latency

Interrupt latency is the delay between the time an interrupt is received and the start of the execution of the ISR. It is a critical factor in real-time systems, where timely response to events is essential. Factors affecting interrupt latency include:

- **Interrupt Disable Time**: The time during which interrupts are disabled, preventing the CPU from responding to new interrupts.
- **Context Switch Time**: The time taken to save the current context and switch to the ISR context.
- **ISR Execution Time**: The time taken to execute the ISR itself.
- **Nested Interrupts**: If higher-priority interrupts are allowed to preempt lower-priority ISRs, this can introduce additional latency.

#### Example of Interrupt Handling in C

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void interrupt_handler(int signum) {
    printf("Interrupt signal received: %d\n", signum); //Avoid using printf in ISR in real systems
}

int main() {
    signal(SIGINT, interrupt_handler);
    while (1) {
        printf("Running...\n");
        sleep(1);
    }
    return 0;
}
```
In this example, the program sets up a signal handler for the `SIGINT` signal (typically generated by pressing Ctrl+C). When the signal is received, the `interrupt_handler` function is called, printing a message to indicate that the interrupt was received. The program continues running in a loop, printing "Running..." every second until interrupted. 

#### Triggering methods

- Level Triggered: The interrupt signal remains active until the interrupt is acknowledged by the CPU. This allows for multiple interrupts to be detected while the CPU is processing an ISR.
- Edge Triggered: The interrupt signal is triggered by a change in state (rising or falling edge). The CPU responds to the change, and the interrupt signal is cleared after the ISR is executed. This method is often used for devices that generate short pulses, such as buttons or sensors.
