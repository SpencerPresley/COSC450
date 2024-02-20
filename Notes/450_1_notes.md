# Overview of Operating System
- What is operating system
- Macroscopic view of computer system
- Computer Structure - Von Neumann Architecture
- Von Neumann Bottleneck
- Instruction Cycle
- History of Computer System
    - First Genereation: Vacuum tubes and plugboards
    - Second Generation: Transitors and Batch System
    - Third Generation: IC and Multiprogramming
    - Fourth Generation: Personal Computer and LSI (VLSI, ULSI)
    - Fifth Generation: Mobile Computers

## What is Operating System?
### Modern Complex Computer System
**Processor**, **Memory**, disk, printer, keyboard, monitor, network interface, other I/O devices. 

**Macroscopic View:**
- Processor
- Memory
- I/O Devices

## A Computer System
### Physical Devices
- IC Chips (CPU, Memory, IO devices, ...)
- Wires
- Power supplies
- CRT (cathode ray tubes)
### Micro-Architecture
- Physical devices are grouped together to form functional units
- Example: registers interal to CPU, a data path containing an ALU
### Machine Language
- Purpose of the data path is to execute some set of instructions. Usually 50 to 300 instruction in the system written by machine code  

### Von Neumann Bottleneck
A limitatin on **throughput** caused by the standard personal computer architecture.
- **Throughput** is a measure of how many units of information a system can process in a given amount of time.  

#### Von Neumann Architecture
- programs and data held in memory
- processor and memory separate, data moves between them

Since **processor calculation speeds are faster than data movement between memory and CPU**, it causes a bottleneck.

### Instruction Cycle
- CPUs main task is to execute instructions
- Therefore, ***Instruction Cycle*** is at the heart of understanding the function and operation of the CPU (controlled by OS)

> Note: Fetch Cycle and Execute Cycle are left and right branches off Instruction Cycle

### Instruction Cycle: Fetch Cycle
- Reads the address of the instruction in (PC) to be executred from memory
- Loads the instruction into the **Instruction Register (IR)**.
- **Program Counter** register **(PC)** is changed to point to the next valid instruction.

### Instruction Cycle: Execute Cycle
- Contents of **IR** are decoded and executed
- Execution may result in various actions depending on type of instruction
- May be a self contained instruction, or could involve iteraction with **memory** and **ALU**

## So again, what is an OS?
2 views, user's and developer's
### User's
- an **Extended Machcine**
    - Consider OS as part of a computer system (extended machine)
    - Since OS provides an interface between user and hardware, computer users could use computer hardware (CPU, Memory, I/O devices) without knowing all the technical details which must be performed. 
### Developer's
- a **Resource Manager**
    - Computer conists of **CPU, RAM, and I/O Devices**
    - OS manages these resources in order to implement an application software through:
        - Process Management
        - Memory Management
        - File Management
        - I/O Management
        - Deadlock Management

## History of Operating Systems
### First Generation: Vacuum Tubes
- 1945 ~ 1955
- Vacuum Tubes
- Plugboards

Used **vacuum tubes** to build calculating engines  
All programs written in **Machine Language** by **wiring up plugboards** to control machine's basic functions  
A programmer signed up for a block of time to use the **Machine Room** and would insert **Plugboards** and wait for a calculation
**NO OPERATING SYSTEM: HUMAN OPERATED THE COMPUTER**

- IBM SSEC (1948)
- Speed: 50 multiplications/second
- I/O: plugboard, cards, punched tape
- Memory Type: punched tape, vacuum tubes, relays
- Technology: 20,000 relays, 12,500 vacuum tubes
- Floor space: 25 feet by 40 feet

### Second Generation: Transistor & Batch System
- 1955 ~ 1965
- **Tranistors and Batch System**  

**Transistor** invented in mid 1950s.
- Computer became more reliabe
- **Tubes** replaced with **Transitors** which are **smaller and faster**

To run a job or **Batch System** 
- Programmer write program on **coding paper**
- Punch program on cards, **one card per line of code**
- Brought cards to input room and submit to the operator
    - Card reader reads the card and saves it on magnetic tape
    - Loads it into memory
- If **FORTRAN** compiler needed, operator would bring the compiler from the cabinet and load it into the computer
- Wait for output. The output would be **written to a tape then printer prints the output**

### Third Generation: IC and Multiprogramming
- 1965 ~ 1980  

Maintained two completely different products
- IBM 7094
- IBM 1401  

was expensive for the manufacturers in the **second generation**  
- IBM introduces **IBM System/360**
    - for **scientific and commerical use**
- Idea:
    - All software, including OS, OS/360 had to work on. 
        - Need very complex OS with assembly code

#### IC & Multiprogramming
- IBM System/360 first major computer to use **IC**  
- To imporve CPU utilization uses:
    - **Multiprogramming** technique to save CPU time
    - **spooling** (Simultaeous Peripheral Operation on Line) technique to save CPU time
    - **Time Sharing** system to share CPU time between users using terminal

#### Multiprogramming
- **Technique to save CPU time**
- Multiple jobs (processes) loaded onto RAM and ran concurrently
- When CPU becomes avaialbe, one of the jobs in the **ready queue** are selected by **short term scheduler** which loads its current status from the **process table** to CPU and starts to run
- OS needs to keep each job's current status in **process table (process control block)**

#### Spooling
- **Technique to save CPU time**
- Buffering mechanism or a process by which data is temporarily held as a file to be used and executed by a device, program, or system.
- Example:
    - Network printer
        - Process in which info to be printed is temporarily stored in a file, printing to be carried out later
        - Used to prevent slow printers from holding up the system at critical times, and to enable several computers or programs to share a single printer.
        - When a process wants to print a file, it enters a file name in the **Spooler Directory**
        - Printer **daemon** peridiocally checks **spooler directory** for any files that need to be printed.

### Fourth Generation: LSI & PC
- 1980 ~ Present
- Personal Computer built with **LSI** (**Large Scale IC**), **VLSI**, **ULSI**
- Development of **LSI** circuit (contain thousands of transitors) lead to reduced prices of computers
- User types commands from keyboard
    - CP/M
    - DOS (Microsoft)
- Graphic User Interface (GUI)
    - Apple with GUI
    - Linux: SUSE, Ubuntu, Red Hot, etc
    - Mac: macOS X
    - MS: Windows 95, 98, 2000, XP, Vista, 7, 8, 10, 11, ...

### Fifth Generation: Mobile Computers
- 1990 ~ Present
- First smartphones appeared mid-1990s
- Severl smartphone OS's
    - Symbian OS
    - RIM's Blackberry OS
    - Android OS by Google
    - iOS by Apple

## Operating System Zoo
- Mainframe OS
- Server OS
- Multiprocessor OS
- Personal Computer OS
- Real-time OS
- Embedded OS
- Smart card OS
- Smart phone OS

