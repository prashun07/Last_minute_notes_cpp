

# Embedded Systems – Interview Preparation Notes

---

## 1. What Embedded Systems Experience Do You Have?

In answering this question in an interview, the focus should be on practical exposure. A good response would describe working with microcontrollers or SoCs, writing firmware in C, developing device drivers, handling communication protocols such as UART, SPI, and I2C, debugging hardware using tools like logic analyzers or JTAG, and working with RTOS or Embedded Linux. The explanation should emphasize hardware-software interaction, interrupt handling, memory management, and performance optimization rather than just programming experience.

---

## 2. Difference Between Microcontroller and Microprocessor

A microcontroller is an integrated chip that contains a CPU, memory (RAM and Flash), timers, and peripherals such as UART, SPI, and GPIO on a single chip. It is designed for dedicated control applications. A microprocessor, on the other hand, contains only the CPU core and requires external components such as RAM, storage, and peripherals to function. Microcontrollers are commonly used in embedded applications like washing machines or automotive ECUs, whereas microprocessors are used in systems like computers and smartphones where higher computational power is required.

---

## 3. Address Bus and Data Bus

The address bus carries the memory address from the processor to memory or peripherals, indicating where data should be read or written. Its width determines how much memory can be addressed. For example, a 32-bit address bus can address 4GB of memory.

The data bus carries the actual data between the processor and memory or peripherals. Its width determines how much data can be transferred in a single operation. For instance, an 8-bit data bus transfers one byte at a time.

---

## 4. What is Clock in a Controller? Types of Clocks

A clock in a controller is a periodic signal that synchronizes the execution of instructions and peripheral operations. It defines how fast the processor runs.

Controllers typically have multiple clocks, such as the system clock for CPU operation, peripheral clocks for communication modules, real-time clocks for timekeeping, and external crystal oscillators for accurate timing. Clock frequency directly affects performance and power consumption.

---

## 5. What is an Embedded System?

An embedded system is a dedicated computing system designed to perform a specific function within a larger system. Unlike general-purpose computers, embedded systems are optimized for reliability, efficiency, and real-time operation. Examples include automotive control units, smart appliances, medical devices, and industrial controllers.

---

## 6. Types of Memory and Differences

Embedded systems use volatile and non-volatile memory. RAM is volatile memory used for temporary data storage during execution. Flash memory is non-volatile and stores firmware permanently. EEPROM is another non-volatile memory used for storing small configuration data. Cache memory is used to improve performance by storing frequently accessed data closer to the CPU.

The key difference is that volatile memory loses data when power is removed, while non-volatile memory retains data.

---

## 7. Difference Between OS and RTOS

A general-purpose operating system focuses on maximizing throughput and user experience, but it does not guarantee response time. An RTOS, or Real-Time Operating System, guarantees deterministic response times to events. In RTOS, task scheduling is predictable, making it suitable for time-critical systems such as automotive braking systems or industrial automation.

---

## 8. Basic Protocol Understanding: UART, SPI, I2C

UART, SPI, and I2C are communication protocols used to transfer data between devices.

UART is asynchronous and uses two lines (TX and RX). It does not require a shared clock and uses start and stop bits to frame data.

SPI is synchronous and uses a shared clock along with MOSI, MISO, and chip select lines. It supports high-speed communication but requires more wiring.

I2C is also synchronous and uses two wires (SDA and SCL). It supports multiple devices on the same bus using addressing but is slower compared to SPI.

---

## 9. UART

### 9.1 How Data is Transferred in UART

UART transmits data asynchronously using a start bit, followed by data bits (usually 8), optional parity bit, and one or more stop bits. Both transmitter and receiver must agree on baud rate beforehand.

---

### 9.2 What is Baudrate? Difference Between Bitrate and Baudrate

Baud rate refers to the number of signal changes per second. Bitrate refers to the number of bits transmitted per second. In simple UART without advanced encoding, baud rate and bitrate are typically equal. Common baud rates used are 9600, 115200, and sometimes higher depending on hardware capability.

---

### 9.3 Maximum Baudrate Supported

The maximum baud rate depends on hardware design and clock frequency. Many microcontrollers support up to several Mbps. Practical limits depend on noise, cable length, and system stability.

---

### 9.4 What is USART?

USART stands for Universal Synchronous Asynchronous Receiver Transmitter. Unlike UART, USART supports both asynchronous and synchronous communication modes.

---

## 10. I2C

### 10.1 How to Read/Write Data in I2C

Communication begins with a start condition. The master sends the slave address followed by a read or write bit. The addressed slave acknowledges. Data bytes are transferred, each followed by an acknowledgment bit. Communication ends with a stop condition.

---

### 10.2 Does I2C Support Multi-Master?

Yes, I2C supports multi-master. Arbitration is handled by monitoring the SDA line. If two masters transmit simultaneously, the one sending a high while detecting a low loses arbitration and stops transmitting.

---

## 11. SPI

### 11.1 Does SPI Support Multi-Master?

Standard SPI does not support multi-master because there is no built-in arbitration mechanism. Only one device should generate the clock.

---

### 11.2 Data Transfer in SPI

SPI uses full-duplex communication. The master selects a slave using the chip select line and transmits data via MOSI while receiving via MISO simultaneously.

---

### 11.3 Is There Acknowledgement in SPI?

SPI does not have built-in acknowledgment. It relies on protocol-level confirmation. I2C, however, includes an acknowledgment bit after each byte.

---

## 12. What is DMA?

DMA, or Direct Memory Access, allows peripherals to transfer data directly to memory without CPU intervention. This improves performance and reduces CPU load, especially for high-speed data transfers like ADC or network packets.

---

## 13. What is Interrupt?

An interrupt is a signal that temporarily halts the normal execution flow of the CPU and transfers control to a specific handler function to respond to an event such as data arrival or timer expiry.

---

## 14. What is ISR?

An Interrupt Service Routine is the function executed when an interrupt occurs.

### 14.1 Is It Okay to Sleep in ISR?

No. Sleeping or blocking operations are not allowed in ISR because interrupts must execute quickly and cannot wait.

### 14.2 Can We Pass Parameters or Return Values?

No. ISRs follow predefined function signatures defined by hardware and do not return values in the traditional sense.

### 14.3 Can We Call Any Function in ISR?

Only functions that are safe and non-blocking should be used. Functions that may sleep or allocate large memory should be avoided.

---

## 15. What Happens When an Interrupt Occurs?

The CPU saves the current execution context, switches to interrupt mode, executes the ISR, restores context, and resumes the previous task.

---

## 16. Interrupt Latency and Turnaround Time

Interrupt latency is the time between interrupt occurrence and ISR execution. Turnaround time is the total time taken to complete ISR execution and return to normal operation.

---

## 17. Timers in Microcontroller

Timers are hardware counters driven by a clock. To generate a one-second delay, the timer is configured with a prescaler and counter value such that the total clock cycles equal one second.

---

## 18. Watchdog Timer

A watchdog timer resets the system if software fails to periodically refresh it. It ensures system reliability by recovering from hangs or deadlocks.

---

## 19. What is IoT?

IoT, or Internet of Things, refers to interconnected embedded devices capable of collecting, exchanging, and processing data over the internet.

---

## 20. What is Segmentation Fault?

A segmentation fault occurs when a program accesses memory that it is not allowed to access. Common causes include dereferencing null pointers, accessing freed memory, or exceeding array bounds.

---

## 21. Endianness

Endianness refers to the order in which bytes are stored in memory.

Big-endian stores the most significant byte first.
Little-endian stores the least significant byte first.

A simple C program to check:

```c
int main() {
    int x = 1;
    if (*(char*)&x == 1)
        printf("Little Endian");
    else
        printf("Big Endian");
}
```

---

## 22. Flash Memory: NAND vs NOR

Flash memory is non-volatile storage used to store firmware.

NOR flash provides fast read speeds and supports execute-in-place, making it suitable for code storage. NAND flash offers higher density and lower cost per bit, making it ideal for mass storage but requiring error correction mechanisms.

---

## 23. Difference Between Asynchronous and Synchronous Communication

In asynchronous communication, there is no shared clock between sender and receiver. Synchronization is achieved using start and stop bits. UART is an example.

In synchronous communication, both devices share a clock signal. Data is transferred in coordinated clock cycles, which increases speed and efficiency. SPI and I2C are examples.

Asynchronous communication is generally simpler but slower. Synchronous communication provides higher throughput and better efficiency.

---
