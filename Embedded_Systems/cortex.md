
# ARM Cortex-A Architecture – Interview Notes

---

## 1. What is ARM Cortex-A?

ARM Cortex-A is a series of high-performance application processors designed to run complex operating systems such as Linux and Android. These processors support virtual memory, MMU, privilege levels, and advanced features like out-of-order execution. They are commonly used in smartphones, embedded Linux systems, networking equipment, and automotive infotainment systems.

Unlike Cortex-M (microcontroller profile), Cortex-A processors are designed for rich OS environments and support full virtual memory systems.

---

## 2. ARM Cortex-A Exception Levels (EL0–EL3)

ARMv8-A architecture defines four privilege levels called Exception Levels:

* **EL0** – User mode (applications)
* **EL1** – Kernel mode (Operating System)
* **EL2** – Hypervisor mode (Virtualization)
* **EL3** – Secure Monitor mode (TrustZone)

When a user application makes a system call, the CPU transitions from EL0 to EL1. In virtualized systems, the hypervisor runs at EL2. Secure firmware runs at EL3.

This layered privilege model enables secure execution and virtualization support.

---

## 3. What Happens During a System Call in Cortex-A?

When a user-space program invokes a system call, it executes the `SVC` (Supervisor Call) instruction. This triggers an exception that causes the processor to switch from EL0 to EL1.

The CPU performs the following:

* Saves current program counter and processor state.
* Switches to kernel stack.
* Executes the exception handler in EL1.
* After handling, restores context and returns to EL0.

This controlled transition ensures security and isolation between user applications and the kernel.

---

## 4. What is MMU in Cortex-A?

The Memory Management Unit (MMU) translates virtual addresses to physical addresses. Cortex-A processors use a multi-level page table mechanism.

The MMU provides:

* Virtual memory support
* Process isolation
* Memory protection
* Cache control attributes

Without MMU, modern operating systems like Linux cannot run.

---

## 5. Explain Virtual Memory in ARM Cortex-A

Each process in Linux runs in its own virtual address space. The MMU maps these virtual addresses to physical memory using page tables.

For example, two processes can both use address `0x40000000`, but internally they map to different physical memory locations.

This ensures isolation and security.

---

## 6. What is TLB?

The Translation Lookaside Buffer (TLB) is a cache that stores recent virtual-to-physical address translations.

Without TLB, every memory access would require walking the page table, which is slow.

When page tables are modified, TLB invalidation (TLBI) instructions are used to maintain consistency.

---

## 7. Cache Architecture in Cortex-A

Cortex-A processors typically have:

* L1 Instruction Cache
* L1 Data Cache
* L2 Unified Cache
* Optional L3 Cache

Caches improve performance by storing frequently accessed data closer to the CPU.

Cache coherency between multiple cores is maintained using hardware protocols.

---

## 8. What is Cache Coherency?

In multi-core Cortex-A systems, each core has its own cache. If one core modifies a memory location, other cores must see the updated value.

Cache coherency protocols ensure data consistency across cores.

This is critical for SMP Linux systems.

---

## 9. What is SMP in Cortex-A?

SMP stands for Symmetric Multi-Processing. In Cortex-A multi-core systems, all cores share memory and are treated equally by the OS.

Linux scheduler distributes tasks across cores dynamically.

---

## 10. What is TrustZone?

TrustZone divides the system into:

* Secure World
* Normal World

Secure world handles cryptographic keys and sensitive operations. Normal world runs Linux.

Switching between worlds is handled at EL3.

---

## 11. What is Stage-1 and Stage-2 Translation?

Stage-1 translation is performed by the operating system (EL1) to translate virtual addresses to intermediate physical addresses.

Stage-2 translation is performed by the hypervisor (EL2) to translate intermediate physical addresses to real physical addresses.

This is required for virtualization support.

---

## 12. What is ARM Generic Interrupt Controller (GIC)?

The GIC manages interrupts in Cortex-A systems.

It handles:

* Interrupt prioritization
* Distribution to CPU cores
* Masking and enabling

In Linux systems, interrupt handlers are registered at EL1.

---

## 13. What is the Difference Between Cortex-A and Cortex-M?

Cortex-A supports:

* MMU
* Virtual memory
* Linux OS
* High-performance computing

Cortex-M supports:

* No MMU (usually)
* RTOS or bare-metal
* Deterministic real-time behavior
* Lower power

Cortex-A is used in application processors, Cortex-M in microcontrollers.

---

## 14. What Happens During Boot in Cortex-A?

Boot flow in Cortex-A:

1. CPU starts at reset vector.
2. Executes ROM code.
3. Loads bootloader (e.g., U-Boot).
4. Bootloader loads kernel and Device Tree.
5. Kernel initializes MMU, caches, scheduler.
6. Kernel starts init process.

Exception level transitions during boot typically move from EL3 → EL2 → EL1.

---

## 15. What is Out-of-Order Execution?

Cortex-A processors execute instructions out of order to improve performance.

Independent instructions may execute before earlier instructions if data dependencies allow it.

However, memory barriers are required when strict ordering is needed.

---

## 16. What Are Memory Barriers?

Memory barriers ensure ordering of memory operations.

Common types:

* DMB (Data Memory Barrier)
* DSB (Data Synchronization Barrier)
* ISB (Instruction Synchronization Barrier)

They are critical in multi-core programming to prevent reordering issues.

---

## 17. What is Endianness in ARM?

ARM supports both little-endian and big-endian modes.

Most modern ARM systems use little-endian format.

Endianness determines byte storage order in memory.

---

## 18. What is Device Tree in ARM?

Device Tree is a data structure describing hardware layout.

It replaces hardcoded board files.

It defines:

* Memory regions
* CPU cores
* Peripherals
* Interrupt mappings

Kernel reads the Device Tree during early boot.

---

## 19. What is Hypervisor in Cortex-A?

Hypervisor runs at EL2 and manages virtual machines.

It controls:

* Virtual CPU scheduling
* Memory virtualization
* Interrupt virtualization

KVM in Linux uses EL2 for virtualization.

---

## 20. Why Cortex-A is Suitable for Embedded Linux?

Cortex-A supports:

* MMU for virtual memory
* Multi-core SMP
* Advanced interrupt handling
* Hardware virtualization
* Large address space

These features make it ideal for embedded Linux systems requiring high performance and flexibility.

---
