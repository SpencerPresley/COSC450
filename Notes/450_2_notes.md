# **COSC450 Notes for COSC450_2.pdf**
# COSC450_1.pdf Review
**What is Operating System-extended machine?**
>Resource Manager  

**Computer System (Macro View)**
>- CPU
>- ram
>- I/O Devices

**Instruction Cycle (Von Neumann)**
>- Fetch
>- Decoding
>- Execution

**History of OS**
>**First Generation - Vacuum Tubes**
>- Human Operates Computer  

>**Second Generation - Transistors**
>- NO Multiprogramming
>- Batch System  

>**Third Generation - IC (Integrated Circuits)**
>- Multiprogramming
>- Time Sharing
>- Spooling  

>**Fourth Generation - LSI, VLSI, ULSI**
>- Personal Computer
>- **LSI:** *Large-Scale Integration*. Process of integrating embedding **thousands of transitors** on a single silicon semiconductor microchip. 
>- **VLSI:** *Very Large-Scale Integration*. Process of **creating an Integratedd Circuit (IC)** by combining **millions or billions of MOS transistors** into a single chip. Began in the 70s, enabled the development of complex semiconductor and telecommunication technologies.
>- **ULSI:** *Ultra Large-Scale Integration*. Process of integrating or embedding **millions of transistors on a single silicon semiconductor microchip**. Conceived in the **80s**, was used in developing of better processor microchips, specifically for intel 8086 series.

>**Fifth Generation - VLSI, ULSI**
>- Mobile Computer (Smartphone)  

# Preview
## Computer System Architecture
- CPU
- Interrupt and Implementation
- Memory Hierarchy
- Input/Output (I/O) Devices
- Buses
    - Parallel
    - Serial
- Single-Processor System
- Multi-Processor System  
## Multi-Processor System Types
- Single-core Chip
- Multi-core Chip
- Symmetric Multi-Processing (**SMP**)
    - Share one physical memory
- Non-Uniform Memory Access (**NUMA**)
    - Each processor has **local memory** but **share one physical address space**
- Clustered System
    - Asymmetric Clustering
        - One machine in **hot standby mode** monitoring the **active server** (active machine)
        - Other machine is the **active server** (active machine) which is doing the tasks.
        - If the machine in **Hot Stanby Mode** detects an issue with the **active server** while monitoring it the machine in **HTM** will take over the task.
    - Symmetric Clustering
        - Machines **monitor each other**
            - Machine A works on its tasks and monitors machine B, machine B works on its tasks and monitors machine A.
            - If machine B fails Machine A will take over its tasks and work on both its tasks and machine B's tasks.
            - Important to ensure that each machine has the capacity (resources) to handle both machines tasks in the even one fails and the other machine takes over tasks of both machines

# Computer System Architecture
>Modern, general-purpose computer system consists of one or more CPUs and a number of device controllers connected through a common **bus** that provides access between **I/O devices** and **Shared Memory**.  
>- CPU(s)
>- Memory(s)
>- I/O Device(s)  

>Each **Device Controller** is in charge of a specific type of **I/O Device**.
>Depending on the controller, it's possible more than one device could be attatched.
>- Ex: One systems USB port can connect to a USB hub, where several devices can be connected to the hub.  

>**Device Controller** maintains a **local buffer storage** as well as a set of **special-purpose registers**.  
>The **Device Controller** is responsible for moving **data** between the **peripheral devices** it controls and the device controller's **local buffer storage**  

>Each **I/O device** has a **Device Driver** for each **Device Controller**. Device driver becomes part of OS.

**Summary**  
Modern computers can have 1 or more CPUs and a number of device controllers connected to them via a common bus. The common bus provides access between I/O devices and the memory each CPU shares. 

Each device controller handles a specific type of I/O device. There could be more than one device controlled by a single device controller.  

The device controller has a **local buffer storage** and **special-purpose registers**. These help it to control the operation of the **peripheral devices, hold status information, and move data between the peripheral devices and the local buffer storage.**

Each I/O device has a device driver for each device controller. Through this interaction the device driver integrates and beecomes part of the OS. 

## Computer System Architecture - **CPU**
>CPU is the "Brain" of the computer  

>Each type of **CPU** has specific **instruction sets** which can be used for **executing** each instruction.  

>Accessing **RAM** to fetch an instruction or data takes significantly longer than actually executing the instruction. (**Fetch cycle** is **slower** than **execute cycle**).  
>All **CPUs** contain a set of **registers** as well as a **cache to hold instructions, key variables, and temporary results**.  
- **Program Counter (PC)**
- **Instruction Register (IR)**
- **Data Register**
- **Stack Pointer**
- etc  
>CPU **fetches** instructions **from memory** and **executes** them.  

Basic form of an **Instruction Cycle** executed by CPU:
1. **Fetch** a **instruction** or **data** from **memory** and transfer it to **register**
2. **Decode** the instruction/data
3. **Execute** the instruction/data  

This cycle is repeated until program finishes.

### CPU components
- ALU (Arithmetic Logic Unit)
- Control Unit
- Cache (depends on design of CPU, it could not be integrated and instead somewhere else on the motherboard)
- Registers
    - General Registers
        - Instruction Registers
        - Data Registers
    - Program Counter (PC):
        - >Saves address of next instruction (virtual address)
    - Stack Pointer:
        - >Address of the top of stack for currently running process
    - Program Satus Word (PSW):
        - >Saves control info for each process
            - Condition code bits
            - CPU priority
            - Mode (User, Kernel)
    
>OS **must know the content of each register for a process.**  
>**When a process stops running on CPU** by changing state (running, ready, blocked) from **running to ready** or **running to blocked** the **OS saves content of each register (snapshot of CPU)** for the process in **Process Table (or Process Control Block)** which need to be used when program returns to a **running state** in order to pick up from where it left off and eventually finish.  
>**Summary:**  
>CPU schedulers will not let one process run forever or even until it's finished, it will move it from running to ready to give other processes time. Additionally when a program reaches a point where it needs to wait for some input (say a cin>> from the user) the CPU will move the program from running to blocked, once it gets it's input it moves back to ready and waits for scheduler to give it CPU time.  
><br>In order for a process to leave a running state to either ready or blocked and return to the CPU later to continute it's execution the OS must be able to remember what the contents of the CPU registers were for that process. The way this is done is that when a process moves from ready to blocked or ready the OS saves a snapshot of the CPU before it moves the process to blocked or ready, it saves this snapshot of the CPU running the process to the "Process Table", when the process moves back to ready the OS loads the snapshot from the process table back onto the CPU so that the process can pick up from where it left off.
>- **Process Table** is a **data structure** that contains **information** about each **process** in the **system**.
>- **Process Control Block (PCB)** is a **data structure** that contains **information** about each **process** in the **system**.  

>**CPU Performance** can be improved via using **Pipelined Design** for **fetch**, **decode**, and **execute** process. This is because fetching data take more time than execution.  

## Computer System Architecture - **Interrupt**
>When an **I/O device** is ready to receive or send data through the **bus**, it **intterupts** OS by sending a **signal**.
>- **Interrupt** is a **signal** sent to the **CPU** to **stop** what it's doing and **execute** a **specific routine**.
>- For **I/O** operation, the **device driver** loads an instruction (read or write) to its **device controller's instruction register (IR)** and then **sends an interrupt signal** to the **CPU**.
>- The controller starts the transfer of data from teh device to its **local buffer storage**
>   - this is a read operation
>   - read: transfer dtat from bus to local buffer
>- Once **transfer of data** is complete, check for any error then the **device controller** informs the **device driver** that its **ready to transfer**.
>- **Device Driver** then gives control to other parts of OS  

Hardware **Interrupts** are **signals** sent to the **CPU** by **hardware devices** to **interrupt** the **current program** and **transfer control** to the **device driver**.  
- **Device Driver** is a **software** that allows **higher-level computer programs** to communicate with a **hardware device**.

Hardware may trigger an interrupt at any time by sending a signal to the CPU, usually via the **system bus**.
  
>**Interrupts** are a **key part** of how **OS systems** and **hardware** interact.  

When the **CPU** is **interrupted**, it stops what it's doing and **immediately** transfers from **execution** to a fixed location where the **service routine for the interrupt is located.**
>- This **fixed location** is often referred to as the **interrupt vector table**, which **contains** the **addresses of all the service routines**.  

The interrupt service routine executes; on completion, the CPU resumes the interrupted computation.  

Since interrupts must be handled as **ASAP**, the OS maintains a **table of pointers (interrupt vector table)** in low memory for holding addresses of interrupt service routines.  

### Interrupt Implementation
>Hardware has a **CPU wire** called the **Interrupt-Request line** that senses an interrupt after executing every CPU instruction.  

>When **CPU Detects** that a controller has triggered an **interrupt signal** on the **interrupt-request line**, **it reads the intterupt number** and ***jumps* to the interrupt-handler routine** by using the **interrupt number as an index** into the **interrupt vector table**.  

>A **interrupt handler** starts execution and might create output and return the CPU to the execution state prior to the interrupt.
>1. A device controller raises an interrupt by asserting a signal to the interrupt request line
>2. CPU catches the interrupt and dispatches it to the interrupt handler
>3. The interrupt handler clears the interrupt by servicing the device
>4. CPU returns to the state prior to the interrupt.  

>Most CPUs have two interrupt request lines  
>1. Non-maskable interrupt line (NMI) - Reserved for event such as **unrecoverable hardware error**
>2. Maskable interrupt line (MI) - used by **device controllers** to **request service**  

**Summary**
- **Device Controllers** and **Hardware Faults** raise interrups.
- To ensure the most **urgent tasks** get completed first, modern computers use a system of **interrupt priorities**
- Since interrupts are heavily used for time-sensitive processing, efficient interrupt handling is required for good system performance.  

## Computer System Architecture - **Memory**
>**CPU** can load instructions only from **RAM**, so any programs must first be loaded into memory (RAM) from the secondary memory (HDD, SSD, NVME, etc) to run.  

**Storage Hierarchy**
- Registers in CPU (volatile)
- Cache in CPU (volatile)
- Main Memory: RAM (volatile)
    - DRAM (Dynamic RAM) - Built with capacitors
    - SRAM (Static RAM) - Built with logic gates
- Secondary Memory: HDD, SSD, NVME, etc. (non-volatile)
    - HDD (Hard Disk Drive) - Rigid metal or glass platters covered with magnetic recording material
    - SSD (Solid State Drive) - floating-gate transistors
    - Magnetic Tape
    - USB  

## Computer System Architecture - **Input/Output Device**
>Large portion of an OS's code is dedicated to I/O management. This is because of its importance to the reliability and performance of a system and teh varying nature of the devices connected to it.  

>Form of interrupt-drive I/O is fine for moving small quantities of data, but produces high overhead when used for bulk data transfer.  

>Solution to the high overhead is to use **DMA (Direct Memory Access)**. 
>- DMA controller handles I/O independent of CPU.  

>DMA is a capability provided by come computers **bus architecture**. It allows data to be sent directly from an attatched device (such as a disk drive) to the memory.  

>This frees the CPU from handling data transfer, thus speeding up the overall computer operation.  

### OS controlls all I/O devices by:
- Issuing commands (read/write) to Devices  
- Detecting and Catching Interrupts from Devices
    - when devices are ready to send or receive data they send an interrupt signal.
- Handle the erro.
- OS also provides and interface between the devices and the rest of the system.  

### Most I/O devices consist of:
Mechanical Components (moving parts)
- Device itself (keyboard)  

Electronic Components
- Device Controller (adapter)
    - Connects the device to the bus
    - Contains the logic to control the device
- On a personal computer, it takes the **form** of a **chip** on the **parent board** or printed **circuit card** than can be **inserted into a PCI expansion slot**.  

Device Driver (Software)
- Standard interface between device controller and OS  

## Computer System Architecture - Buses  
The **bus** in a PC is a **collection of wires** that connect the **CPU** to the **memory** and **I/O devices**.  
- **Bus** is a **shared communication channel** that connects the **CPU** to the **memory** and **I/O devices**.  
- **Bus** is a **collection of wires** that connect the **CPU** to the **memory** and **I/O devices**.  
- **Bus** is a **shared communication channel** that connects the **CPU** to the **memory** and **I/O devices**. 

In short the **bus** in a computer system is a **shared communication channel** that connects the **CPU** to the **memory** and **I/O devices**. I.e., the **pathway between the CPU and peripheral devices.**

**Bus Types**
- Parallel
    - Use slots on motherboard
    - provide multiple lines for data (32 bits, 64 bits) between CPU and peripheral card.
    - Cards plug into thee bus inside the cabinet
- Serial
    - exernal ports
    - cable that plugs into them can connect to multiple devices

**More on Bus:**
>**Bus Speed and Bandwidth**
>- **Speed** of a bus refers to how fast data can be transferred across the bus, typically measured in MHz or GHz.
>- **Bandwidth** is the maximum amount of data that can be transmitted in a fixed amount of time, usually measured in bits per second (bps) or bytes per second.
    
>**Bus Arbitration**
>- In systems with multiple devices that may need to use the bus simultaneously, a method of **bus arbitration** is necessary.
>- This can be done through **daisy chaining** or using a **bus arbiter** chip to manage access to the bus, ensuring that only one device uses the bus at a time.
    
>**Bus Standards**
>- Over the years, several bus standards have been developed to cater to the needs of the evolving computer hardware.
>- Examples include **PCI (Peripheral Component Interconnect)**, **PCIe (PCI Express)**, **USB (Universal Serial Bus)**, and **Thunderbolt**.
>- Each standard differs in terms of speed, bandwidth, and physical form factor, making them suitable for different types of connections and devices.  

## Computer System Architecture - Parallel Buses
**Advantage**
- Fast data communication between CPU and devices  

**Disadvantage**
- Short communication distance due to **crosstalk** between parallel lines.
- Costs more due to the multiple liles and pins to connect  

**Parallel Buses:**
- **Peripheral Component Interconnect (PCI)**
    - Available in 32 and 64 bit version
    - Most popular bus architecture
- **Accellerated Graphics Port (AGP)**
    - Designed for faster screen display
    - Available in 32 and 64 bit versions
    - If used, only one AGP slot on motherboard

## Computer System Architecture - Serial Buses
**Advantage**
- Supports long distance communication
- Costs less than parallel buses  

**Disadvantge**
- Slower data communication between CPU and devices  

**Serial Buses:**
- Universal Serial Bus (USB)
    - Supports up to 127 devices on one USB port
    - First USB version designed for low-speed peripherals
- FireWire (IEEE 1394)
    - Designed for high-speed peripherals
    - Supports up to 63 devices on one FireWire port
    - Mostly used for digital cameral connections

## Computer System Architecutre - Parallel Buses: Earlier Version
**Parallel Buses (Earlier Version)**
- Industry Standard Architecture (ISA)
    - from original PC
    - 8-bit bus
    - originally known as PC bus then XT bus
    - later extended to 16 bits and became AT bus
- Extended Industry Standard Architecture (EISA)
    - 32-bit bus
    - extension of ISA
    - created by major venders to counter IBM's Micro Channel
    - EISA slots accepted both EISA and ISA cards
    - clock speed still at the slow ISA rate
- Micro Channel Architecture (MCA)
    - In '87 IBM switched to
        - 32-bit Micro Channel with PS/2 line
    - later added back ISA
    - eventually gave up Micro Channel for PCI
- VESA Local Bus (VL)
    - 32-bit
    - introduced during 486 era
    - offered higher speed than ISA
    - gave way to PCI  

## Computer Architecture - Single Processor Systems
- Computer System with one CPU that had one Core
- **Core** is the compoment that executes instructions and registers for storing data locally.
- One main cpu with its core is capable of executing a general-purpose instruction set, including instructions from processes
- These systems have other special-purpose processors that are designed to handle specific tasks.
    - disk, keyboard, graphic controllers
- Special-Purpose Processors run a limited instruction set and DO NOT run processes.
- OS manages special-purpose processor via:
    - sending info about their next task
    - monitoring their status
    - Ex: DMA
    - Ex: a processor in the keyboard to convert the key strokes into code to be sent to CPU  

## Computer System Architecture - Multi-Processor Systems
**Primary Advantage**
- Increased Throughput
    - Expect to get more work done in less time by increasing number of processors

Speed-up ratio with N processors is less than N
- Speed-up ratio is the ratio of the time it takes to run a program on a single processor to the time it takes to run the program on N processors.
- Adding processors to a system does not always increase the speed of the system. It is not a linear increase, and sometimes is not an increase at all, could decrease system performance due to overhead/ lack of resources.  

Multiple computing cores reside on a single chip  

Multicore systems can be more efficient than multiple chips with single cores as on-chip communication is faster than between-chip communication.  

One chip with multiple cores uses significantly less power than multiple single-core chips, an important issue for mobile devices as well as laptops, as well as a cost factor.  

### Multi-Processor System Types**
**Symmetric Multiprocessing (SMP)**
- Each CPU has:
    - Own set of registers
    - Cache memory (possibly)
- All CPUs
    - share a physical memory over the system bus
- Virutally all modern OS's support mulitcore SMP systems
- SMP systems are used in servers, desktops, and laptops
- Adding CPUs to a multiprocessor system will increase computing power **TO A POINT**
    - Once too  many CPUs are added:
        - contention for system bus becomes a bottleneck
        - contention for memory becomes a bottleneck
        - leads to performance to **DEGRADE**  

**Non-Uniform Memory Access (NUMA)**
- Each CPU has:
    - own local memory
        - accessed via a small and fast local bus
- CPUs are connected via:
    - shared system interconnect
        - all CPUs share the same physical address space
        - all CPUs share the same I/O
        - all CPUs share the same system software
- Advantage:
    - CPU accessing local memory is fast and has no contention over the system interconnect
- Possible Drawback:
    - increased latency for CPU accessing remote memory
        - CPU must access the remote memory across system interconnect, possibly leading to a performance penalty
    - OS must be careful about CPU scheduling and memory management to reduce overhead

### **Clustered Systems**
Two or more individual systems are connected locally via LAN. They are **treated as a single system** by the OS. Loosely coupled.  

Clustering usually used to provide:
- high availability service
    - service continues even if one more systems in the cluster fail  

High-availability service can be obtained by adding a level of redundancy to the system.
- Layer of cluster software runs on the cluster nodes
- Each node can monitor one or more of the others (over the network)
- If monitored machine fails, the monitoring machine can take ownershiop of its storage and restart teh applications that were running ont he failed machine  

**Clustered System Types**
Asymettric Clutering (AS)
- one machine in hot standby mode
- one machinie in active server mode
- hot standby machine monitors the active machine, if active server fails it takes over  

**Symmetric Clustering (SC)**
- All machines are active
- All machines monitor each other
- If one machine fails, the others take over its work  

Since a cluster consists of several computers connected via a network, clusters can also be used to provide high-performance computing environments.  

To take advantage of the cluster structure, applications must be written specifically to do so. A program is divided into separate components (using thread) that run in paralllel on indiviudal cores in a computero r computers in a cluster.  

# OS Operation  
## OS Operation - Multiprogramming
- System keeps several processes in memory simultaneously and provide concurrently for processes
- OS (short term scheduler) selects one of the processes in the ready queue and lets it use an availalbel CPU to execute its instructions
- Eventually the process may have to wait for something (other process, I/O input, etc) to complete 
- Multiprogramming increases CPU utiliziation, as well as keeping users satisfied by organizing programs so that the CPU always has one to execute  

## OS Operation - Multitasking
- Logical extension of multiprogramming
- allows user to perform  more than one computer task at a time
- OS able to keep track of where you are in these tasks and go from one to another without losing info
- Possible to support multitasking without multiprogramming  

## OS Operation - Dual Mode & Multimode Operation
- OS and users share resources, OS must ensure malicious programs cannot cause other programs to execute/behave incorrectly 
- To do this;
    - system has to distinguish between execution of OS code and user code (Kernel mode or user mode)
- Approach taken by most computer systems is:
    - provide hardware support that allows differentiation among various modes of executin
- A bit, called a mode bit, added to hardware of computer to indicate current mode:
    - 0: Kernel Mode
    - 1: User Mode
- When process requests a service via system call, system switches from user to kernel mode to fufill request  

>Dual mode operation provides us with means for protecting OS from errant users or hackers.  
> Protection accomplished via designnating some of the machines instructions that could cause harm so that they can only be executed in kernel mode  
> Concept of modes can be extended to more than two  
- 0: kernel
- 1 & 2: Used for various OS service, rarely used
- 3: User  







