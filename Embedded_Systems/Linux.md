
# Linux & Embedded Boot Process – Interview Notes

---

## 1. What is the Booting Process of the Linux Kernel? Explain Each Stage.

The Linux boot process is the sequence of operations that takes place from the moment power is applied to a system until the operating system becomes fully operational and user-space processes start running. The exact steps vary depending on the architecture (x86 or ARM), but the fundamental stages remain conceptually similar.

On traditional x86 systems, the boot process begins with the BIOS or UEFI firmware. When the system is powered on, the firmware performs a Power-On Self Test (POST) to initialize hardware components such as CPU, RAM, and storage devices. Once the hardware is initialized, the firmware looks for a bootable device and loads the first-stage bootloader from the Master Boot Record (MBR) or EFI partition into memory.

The bootloader, such as GRUB, is responsible for loading the Linux kernel image (commonly named `vmlinuz`) into memory. It also loads the initial RAM filesystem (initramfs or initrd), which contains temporary root filesystem utilities and drivers required during early boot. The bootloader then passes control to the kernel along with command-line parameters.

The Linux kernel begins execution by decompressing itself into memory. It initializes core subsystems such as memory management, process scheduling, interrupt handling, device drivers, and virtual file systems. After initializing the kernel environment, it mounts the root filesystem and starts the first user-space process, traditionally `/sbin/init`, which now is typically `systemd`.

The init process then initializes user-space services and transitions the system into a configured operational state (previously known as runlevels, now systemd targets).

---

## 2. What are Primary and Secondary Bootloaders? Why Do We Need Two?

A primary bootloader is a small program that executes immediately after the processor comes out of reset. It is usually stored in on-chip ROM or internal flash memory. Because on-chip memory is limited in size, the primary bootloader contains only minimal functionality. Its responsibility is typically to initialize essential hardware such as clock systems and basic memory, and then load the secondary bootloader into RAM.

The secondary bootloader is a more feature-rich program stored in external flash, eMMC, SD card, or NAND storage. It performs complete hardware initialization, including DDR memory configuration, peripheral setup, and storage subsystem initialization. It may also provide features such as network boot, secure boot, filesystem support, and environment variable management. U-Boot is a commonly used secondary bootloader in embedded Linux systems.

Two bootloaders are required because early hardware resources are extremely limited immediately after reset. The primary bootloader must be small enough to reside in restricted internal memory, while the secondary bootloader provides full flexibility once external memory becomes available.

---

## 3. Explain the Linux Boot Sequence in ARM Architecture.

In ARM-based embedded systems, the boot process differs from x86 systems. When the processor powers on, execution begins from a predefined reset vector in ROM. This ROM code is embedded within the SoC by the manufacturer and is responsible for selecting the boot source (e.g., SD card, NAND, SPI flash).

The ROM code loads the first-stage bootloader, often called SPL (Secondary Program Loader) or MLO (Memory Loader), into on-chip SRAM. The SPL performs minimal hardware initialization, particularly DDR memory setup. Once DDR is configured, the SPL loads the full-featured bootloader such as U-Boot into external memory.

U-Boot then loads the Linux kernel image into memory, along with the Device Tree Blob (DTB), which describes the hardware layout of the board. It sets up kernel command-line arguments and finally transfers control to the kernel entry point.

The Linux kernel uses the Device Tree to identify hardware components and initialize drivers accordingly. After completing kernel initialization, it mounts the root filesystem and launches the init process.

---

## 4. How Are Command-Line Arguments Passed to the Linux Kernel by U-Boot?

Command-line arguments are passed to the Linux kernel through memory structures prepared by the bootloader. In older ARM systems, this was done using ATAGS. In modern systems, it is done using the Device Tree.

In U-Boot, the boot arguments are defined in an environment variable named `bootargs`. For example:

```
setenv bootargs "console=ttyS0 root=/dev/mmcblk0p2 rw"
```

When the boot command is executed, U-Boot places these arguments into the Device Tree structure or ATAGS and passes a pointer to this structure in a register before jumping to the kernel entry point. The kernel reads these parameters during early initialization in the `start_kernel()` function.

---

## 5. Explain ATAGS.

ATAGS (ARM Tags) were an older mechanism used on ARM systems to pass boot-time information from the bootloader to the kernel. ATAGS included details such as memory size, kernel command-line arguments, and initial RAM disk location.

ATAGS consisted of tagged data structures stored in memory. The kernel parsed these tags during early initialization. However, ATAGS have largely been replaced by the Device Tree mechanism, which provides a more flexible and scalable way to describe hardware configuration.

---

## 6. How and Where Are Command-Line Arguments Parsed in Kernel Code?

When the kernel starts executing, the function `start_kernel()` is invoked. One of the early steps in this function is parsing the boot command line.

The kernel stores the command-line string in a global variable called `boot_command_line`. It then processes the parameters using parsing functions such as `parse_args()` and architecture-specific setup functions like `setup_arch()`.

Each subsystem registers its own parameter handlers using macros such as `__setup()` or `early_param()`, allowing specific command-line arguments to configure kernel behavior.

---

## 7. Which Process or Directory is Responsible for Kernel Decompression During Boot?

The Linux kernel image is typically stored in compressed form to save storage space. During boot, a small decompression routine executes before the main kernel starts.

In ARM systems, this code resides in:

```
arch/arm/boot/compressed/
```

For ARM64 systems, it resides in:

```
arch/arm64/boot/compressed/
```

The decompression routine extracts the compressed kernel image into RAM and then transfers control to the uncompressed kernel entry point.

---

## 8. Difference Between MLO and IPL

MLO (Memory Loader) is a term used in some platforms, particularly TI-based systems, to refer to the small first-stage bootloader responsible for DDR initialization and loading U-Boot.

IPL (Initial Program Loader) is a more generic term referring to the first code executed by the processor after reset. Both perform similar roles, but MLO is a platform-specific naming convention.

---

## 9. What is GRUB Loader?

GRUB (Grand Unified Bootloader) is a bootloader commonly used in desktop and server Linux systems. It supports multiple operating systems and provides a boot menu interface. GRUB loads the Linux kernel into memory, passes command-line parameters, and transfers execution to the kernel.

GRUB is typically used on x86 systems and is not common in embedded ARM systems.

---

## 10. How Does a User Mode Transfer to Kernel Mode?

A transition from user mode to kernel mode occurs when a user-space program requests a privileged operation. This typically happens through a system call.

On ARM architectures, a system call is invoked using the `svc` instruction. The processor switches from EL0 (user mode) to EL1 (kernel mode). The kernel then executes the requested service and returns control back to user space after completion.

Other mechanisms that trigger this transition include hardware interrupts, exceptions, and page faults.

---

## 11. How to Decrease Boot Time?

Boot time can be reduced by optimizing various components of the boot process.

One method is minimizing the kernel configuration by disabling unnecessary drivers and features. Compiling essential drivers directly into the kernel instead of loading them as modules can reduce module loading delays.

Reducing the number of services started by the init system can also significantly decrease boot time. Filesystem optimizations, such as using a read-only root filesystem or lightweight filesystems like squashfs, further improve boot performance.

Bootloader delay timers should be minimized, and unnecessary hardware checks can be disabled. Enabling parallel service startup in systemd can also contribute to faster system initialization.

---

# Linux File Systems – Interview Notes

---

## 1. What is a `/proc` Entry and How is it Useful?

The `/proc` filesystem, commonly called **procfs**, is a virtual filesystem in Linux that provides runtime information about the system and kernel. It does not store actual files on disk; instead, it dynamically generates information from kernel data structures when accessed.

A `/proc` entry is a file or directory inside `/proc` that represents system or process-related information. For example, `/proc/cpuinfo` provides CPU details, and `/proc/meminfo` shows memory usage statistics.

The usefulness of `/proc` lies in its ability to expose kernel internals to user space in a controlled manner. It is widely used for debugging, monitoring system status, inspecting running processes (`/proc/<pid>`), and sometimes configuring kernel parameters. Since it reflects live kernel state, the contents change dynamically based on system conditions.

---

## 2. What is a File System? How is it Created?

A file system is a method and data structure used by an operating system to organize, store, retrieve, and manage data on storage devices. It defines how files are named, stored, accessed, and structured on disk.

In Linux, creating a filesystem involves formatting a storage device or partition with a specific filesystem type. For example:

```
mkfs.ext4 /dev/sda1
```

This command creates an ext4 filesystem on the specified partition. The filesystem organizes disk blocks into structures such as superblocks, inodes, data blocks, and directory entries.

After creation, the filesystem must be mounted to become accessible:

```
mount /dev/sda1 /mnt
```

Once mounted, it becomes part of the global directory tree.

---

## 3. How to Register a File System in Linux Kernel?

In kernel development, registering a filesystem means making the kernel aware of a new filesystem type so it can mount and use it.

This is done by defining a `struct file_system_type` structure and registering it using:

```c
register_filesystem(&my_fs_type);
```

This structure contains information such as the filesystem name, mount function, and flags.

When the module is unloaded, it must be unregistered using:

```c
unregister_filesystem(&my_fs_type);
```

This allows the kernel to dynamically support new filesystems through loadable modules.

---

## 4. What Happens When a File System is Registered?

When a filesystem is registered, the kernel adds it to its internal list of supported filesystems. This enables users to mount the filesystem using the `mount` system call.

Once mounted, the kernel calls the filesystem’s mount function, which initializes superblocks, inodes, and other necessary structures. The Virtual File System (VFS) layer then uses these structures to provide a uniform interface to user applications.

If registration fails, the filesystem cannot be mounted.

---

## 5. What Are Different Types of File Systems? Give Examples.

Linux supports several categories of filesystems.

Disk-based filesystems store persistent data on physical storage devices. Examples include ext4, XFS, and Btrfs.

Network filesystems allow access to files over a network. Examples include NFS and SMB.

Virtual filesystems do not store real data on disk but expose kernel or runtime information. Examples include procfs (`/proc`) and sysfs (`/sys`).

Flash-specific filesystems are optimized for flash memory devices. Examples include JFFS2 and UBIFS.

Temporary in-memory filesystems store data in RAM and are lost after reboot. An example is tmpfs.

Each filesystem is designed based on storage characteristics and performance requirements.

---

## 6. What is VFS? Explain Briefly.

The Virtual File System (VFS) is an abstraction layer in the Linux kernel that provides a uniform interface for different filesystem implementations.

Instead of user applications interacting directly with ext4, NFS, or any other filesystem, they interact with the VFS layer. VFS translates generic file operations such as open, read, write, and close into filesystem-specific implementations.

For example, when a user calls `open()`, the request goes to VFS, which determines which filesystem handles that file and then calls the corresponding filesystem’s implementation.

VFS makes it possible to support multiple filesystem types simultaneously without modifying user-space applications. It ensures portability, modularity, and consistency in file handling across the system.

---
