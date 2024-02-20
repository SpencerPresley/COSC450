# OS as a Resource Manager

Operating System responsible for the following activities regarding process management:
- Creating and deleting both user and system processes
- Scheduling processes and threads on CPUs
- Suspending adn resuming processes and threads
- Providing mechanisms for process synchronization
- Providing mechanisms for deadlock handling
- Provide mechanicsm for process communication  

## Memory Mangement  
For program to be executed:
- Must be mapped to absolute addresses and loaded into memory  

As program executes it:
- Accesses program instructions and data from memory by generating the absolute addresses  

CPU reads instructions from main memory (RAM):
- during fetch cycle
- reads and writes data from main memory (RAM) during the data-fetch cylce (on Von Neumann)  

When Program terminates:
- memory space is declared available for use by other programs
    - other porgrams can be loaded and executed  

**OS responsibilities in regards to memory management**
- Keep track of which parts of memory are currently in use and by which process
- Allocating and deallocating memory space ass needed
- Deciding which processes, or parts of processes, (as well as data) to move into and out of memory (virtual memory)  

## File Management  
OS provides file management system that allows users to:
- Create, delete, and manipulate files and directories
- Map files onto secondary storage
- Backup files onto stable (non-volatile) storage
- Protect files and directories from unauthorized access

this is a logical way to view/store user information. 

OS implements teh abstract concept of a file by:
- managing mass storage media and the devices that control them  

File are normally organized into directories in order to make them easier to use  

If a system supports multi-user (like HS101)  
- OS needs to control which user may
    - access a file and how they may access it (rwx permissions)  

**OS responsibilities regarding file management**
- Creating and deleting files
- Creating and deleteing directories
- Supporting primiitves for manipulating files and diretories.
    - read, write, open, lseek, ...
- mapping files onto mass storage (HDD, SSD, etc)
- Backing up files on stable (non-volatile) storage media  

## Mass-Storage Management  
**OS Responsibilities**
- Mounting and unmounting
- free-space management
- storage allocation
- disk scheduling (in HDD)
- partitioning
- protection  

## Cache Management
**Cache is**:
- hardware or software component that:
    - stores data that could be needed again shortly
    - data stored in cache might e result of an earlier computation or a copy of data stored elsewhere

**Cache hit**:
- occurs when requested data can be found in cache
- occurn a cache miss occurs when it cannot find the data in the cache  

Cache have limited size so cache management is an important design problem.  

Careful selection of cache size and of a replacement politcy (with cache miss) can result in greatly increased performance.  

>Movement of information between levels of a storage hierarchy may be either explicit or implicit depending on the hardware design and the controling operating-system software
>- data transfer from cache to CPU and registers is usually a hardware function, with no OS intervention.
>- Data transfer of data from disk to memory is usually controlled by the OS  

## Deadlock management 
Deadlocks happen because there's a limited amount of resources that processes must share  

Processors are sharing resources for finishing their job.  

Some processes might need more than one resource hold to finish its job
- ex: copy data from disk to USB

**4 conditions that combined cause deadlock**
1. Mutual exclusion
2. Circular Wait
3. Hold and Wait
4. No preemption

**Four stages to deal with deadlock**
- Ignore
- Detection and recovery
- Dynamic Avoidance by careful allocation (banker's algorithm)
- Prevention
    - negating one of the four conditions necessary to cause deadlock  

## I/O
OS has I/O subsystems for managing I/O devices  

I/O Subsystems consist of:
1. Memory-management components
    - buffering
    - spooling
    - caching
2. General device drivers interface
3. Drivers for specific hardware
4. Processor for specific hardware  

OS controls I/O devices via:
- issue commands to devices (read/write)  
- Catch interrupts from devices
- Handle error  

OS also provides interface between devices and rest of system  

## OS Structures
- Monolithic
- Layered
- Microkernel
- VM
- Client-Server Modular
- Exokernels 

## Monolithic  
- written as collection of procedures
- each procedure has well-defined interface and each one is free to ccall any other one
- possible to have some structure   

Possible structure:
1. Main program
    - invoke service ffunctions
2. Service functions
    - carry out system calls
3. Utility functions
    - helps service functions 

## Layered
0. Process management
1. Memory management
2. inter-process commun.
3. I/O management
4. Deadlock management
5. user program
6. sysetm operator process  

## Microkernel
- Achieve high reliability by:
    - Splitting OS into small well-defined modules  

## Virutal Machine
- VM monitory (hypervisor) does multiprogramming by providing several VMs
- Each VM are exact copies of the bare hardware
- Different VM can run different OS  



