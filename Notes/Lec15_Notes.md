# Lecture Slide 15 Notes  

## Review 
- Page Table with Hardware Support
  - Translation Look-Aside Buffer
- Shared Pages
- Page Table Structure
  - Multi-Level Page Table
  - Hashed Page Table
  - Inverted Page Table

## Preview 
- Demand Page and Page Fault
- Free-Frame List
- Performance of Demand Page with Page Fault
- Swapping with Paging
- Copy-on-Write between Parent and Child
- Page Table Entries
- Page Replacement Algorithms
  - Optimal Algorithm
  - Not Recently Used
  - First In First Out
  - Second Change
  - The Clock Page Replacement
  - Least Recently Used

## Demand Page & Page Fault 

<u>**Virtual Memory**</u> and <u>**Demand Paging**</u> are memory management techniques used in operating systems.  

<u>**Demand Paging</u> is a type of *swapping* done in *virtual memory systems*.  

While a process is executing, some pages will be in memory, and some will be in secondary storage.  

For a reference, the system checks whether a corresponding page is in the memory or not.  

<u>**Page Fault**</u> is a situation such that process tries to access a page that was NOT brought into memory.  

### The Process of Handling a Page Fault 

1. Check the location of the referenced page in the page table
2. If a page fault occurred, call the operating system to fix it. 
3. Using the frame replacement algorithm, find the frame location.
4. Read the data from the secondary memory to memory.
5. Update the page map table for the process.
6. The instruction that caused the page fault is restarted when the process resumes execution.  

### Continuing

The operating system sets <u>the instruction pointer to the first instruction of the process</u> (which is on a page in secondary memory) before the process is running, <u>the process immediately faults for the page</u>.  

**Pure Demand Paging**
- After this page is brought into memory, the process continues to execute, faulting as necessary until every page that it needs is in memory.
- At that point, it can execute with no more faults (never bring a page into memory until it is required).  

Any given simple instruction could access multiple pages which might cause multiple page faults. 

Consider a **three-address** instruction such as **ADD** content of **A** to **B** and placing the result in **C**
- Fetch and decode instruction **ADD**
- Fetch **A**
- Fetch **B**
- Add **A** and **B**
- Store result in **C**

What if A and B are located in the same page but not C? This instruction may result in two page faults.  

## Free-Frame List  

When a **page fault** occurs, the operating system must bring the desired page from secondary storage into a free page frame in main memory.  

What if there is no free page frame? The operating system needs to exert extra effort to create a free page frame.  

To resolve page faults with o free page frame, <u>most operating systems maintain a **free-frame** list</u>, a pool of free frames for satisfying such requests.  

## Performance of Demand Paging with Page Fault 

Steps for performance of **Demand Paging** with **Page Fault**
1. Trap to the operating system (control change from a process to kernel)
   * OS saves the process' status in process table
2. Check if there was or wasn't a page fault via checking the page table for the process.
3. Check that the page reference was legal and determine the location of the page on the disk (secondary memory)
4. Issue a read from the disk (secondary memory) to a free frame
5. Correct the page table. (Now the process is ready to implement)
6. Restore the process status
7. The process starts to run the instruction

**Three major task components of the page-fault service time:**
1. Service the page-fault interrupt
   * Save the process status
   * Find free page frame
   * If there's no free page frame, decide the victim to be removed based on replacement algorithm
2. Read in the page from secondary memory to main memory
3. Restart the process

In case HDD (Hard Drive Disk)
- Page switch time about 8 milliseconds
- A memory access time of 200 nanoseconds
- Page-Fault rate = p
<br>  
<br>  
>$EffectiveAccessTime=(1-p)*200+8,000,000*p=200+7,999,800*p$

<br>

- Effective Access Time is directly proportional to the page-fault rate p

<u>**Effective Access Time** is directly proportional to the **page-fault rate p**</u>.  

It is important to keep the page-fault rate low in a demand-paging system. Otherwise, the effective access time increases, slowing process execution dramatically.  

An additional aspect of demand paging is the handling and overall use of **Swap Space**.  

I/O to swap space is generally faster than that to the file system.  

It is faster because **swap space** is allocated in much larger blocks, and file lookups and indirect allocation methods are not used.  

### Options to improve paging throughput with swap space 

- Copy and entire file image into the swap space at process startup
  - All demand paging from swap-space. Overheads for start-up to copy image into the swap space.
- Initially, demand-page from the file system but to write the pages to swap space as they are replaces. (Linux or Windows)

## Swapping with Paging 

Process instructions and the data must be in memory to be executed. However, a process, or a portion of a process, can be swapped temporarily out of memory to a backlog store and then brought back into memory for continued execution.  

Swapping makes it possible for the total physical address space of all processes to exceed the real physical memory of the system.  

Standard swapping involves moving entire processes between main memory and a backing store.  

This is no longer used in contemporary operating systems. It's not used as the amount of time required to move the processes between memory and the backing store is prohibitive. 

Most systems, including Linux and Windows, now use a variation of swapping in which pages of a process-rather than an entire process.  

Linux uses virtual memory, <u>so that disk area (swap space) is used as an extension of physical memory for temporary storage when the operatign system tries to keep track of processes requiring more physical memory than what is available</u>. When this happens the swap space is used for swapping and paging.  

## Copy-on-Write (COW) between Parent and Child  

Traditionally, `fork()` worked by creating a copy of the parent's address space for the child, duplicating the pages belonging to the parent.  

Since many child process runs different program by invoking `exec()` system all, copying of the parent's address space may be unnecessary.  

<u>Copy-on-Write (COW) idea works by allowing the parent and child processes to initially share the same pages.  

These shared pages are marked as **copy-on-write** pages, meaning that if either process writes (modifies) to a shared page, a copy of the shared page is created.  

>When process P1 create a child process P2, by calling `fork()`, pages are shared between them.  
>When P2 tries to modify page C in RAM, os finds out free page frame and copies page C, then P2 can modify the space.  

When a child is created by calling `vfork()`, the parent process is suspended and the child process use the address space of the parent.  

Since `vfork()` does not use **copy-on-write (COW)**, if the child process modifies any pages of the parent's address space, the modified page will be visible to the parent once it resumes.  

## Page Replacement Algorithms 

When a page fault occurs and there's no free frame, OS needs to execute the following steps:  
1. Choose a victim page currently allocated in a page frame.
2. If the page has been modified in the memory, rewrite it to the disk (secondary memory or swapping area in secondary memory)
3. A page is allocated into the page frame which was used by the victim page
4. Change page table

First step is dependant on replacement algorithm.  

If no frames are free, two page transfers (one for the page-out and one for the page-in) are required.  

A process's memory access can be characterized by a list of page numbers.  

This list is called the **reference string (sequence of page numbers)**.  

A paging system can be characterized by three items
1. The reference string of the executing process
2. The page replacement algorithm
3. The number of page frames available in memory for a process

## Optimal Algorithm | Page Replacement Algorithms 

- Replace the page that will <u> not be used for the longest period of time</u>.
- Optimal algorithm always guarantees the lowest possible page-fault rate for a fixed number of frames
- Unfortunately, the optimal algorithm is difficult, if not impossible, to implement, since it requires future knowledge of the reference string.  


## Not Recently Used (NRU) | Page Replacement Algorithms 

When page fault occurs, the OS inspects all the pages and classifies them into four groups based on the page table information (Modified bit, Reference bit).  
- Class 0: Not referenced, not modified
- Class 1: Not referenced, modified
- Class 2: Referenced, not modified
- Class 3: Referenced, modified  

The NRU algorithm removes a page at random from the lowest numbered non-empty class.  


## First In First Out (FIFO) | Page Replacement Algorithms 

- **Replace the oldest page frame**
- The FIFO algorithm is easy to understand and easy to implement.
- However, its performance is not always good.  


## Second Chance | Page Replacement Algorithms 

A simple modification of FIFO that avoids the problem of throwing out a heavily used page. The modification is to inspect the Reference bit of the oldest pages.  

If the oldest page's R = 0, it is old and not referenced, so it's removed from the page frame.  

If the oldest page's R = 1, it is old but referenced. So set R = 0 and set the page from oldest to newest page.  


## The Clock Page Replacement | Page Replacement Algorithms 

Similar with second change, but it keeps all the page frames on a circular list.  

When page fault occurs, the page the hands is pointing to is inspected.  

Action taken depends on the reference bit R.
- If R = 0, evict the page
- iF R = 1, clear R and advance hand  


## Least Recently Used (LRU) | Page Replacement Algorithms 

Replace the page that <u> has not been used for the longest period of time</u>.  

This is the optimal page replacement algorithm looking backward in time.  

LRU algorithms works quite good but it may require substantial hardware assistance to keep track of the information.  

LRU can be implemented by:
- Counter: save the reference time
- Stack: keep a stack of page number  

