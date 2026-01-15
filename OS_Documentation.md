# Operating Systems Documentation

## Table of Contents
1. [Introduction to Operating Systems](#introduction-to-operating-systems)
2. [Functions of an Operating System](#functions-of-an-operating-system)
3. [Types of Operating Systems](#types-of-operating-systems)
4. [Process Management](#process-management)
5. [Memory Management](#memory-management)
6. [File System Management](#file-system-management)
7. [I/O Management](#io-management)
8. [Scheduling Algorithms](#scheduling-algorithms)
9. [Deadlocks](#deadlocks)
10. [Synchronization](#synchronization)

---

## Introduction to Operating Systems

An **Operating System (OS)** is system software that acts as an intermediary between computer hardware and software applications. It manages hardware resources and provides services for computer programs.

### Key Characteristics:
- **Resource Manager**: Manages CPU, memory, storage, and I/O devices
- **Interface Provider**: Provides an interface for users and applications to interact with hardware
- **Program Executor**: Facilitates the execution of user programs
- **Error Handler**: Detects and handles errors in the system

---

## Functions of an Operating System

### 1. Process Management
- Creating and deleting processes
- Suspending and resuming processes
- Providing mechanisms for process synchronization and communication

### 2. Memory Management
- Keeping track of which parts of memory are currently being used
- Allocating and deallocating memory space as needed
- Deciding which processes to load into memory

### 3. File System Management
- Creating and deleting files and directories
- Supporting primitives for manipulating files and directories
- Mapping files onto secondary storage
- Backing up files on stable storage media

### 4. I/O System Management
- Managing buffer caching system
- Providing general device-driver interface
- Providing drivers for specific hardware devices

### 5. Secondary Storage Management
- Free-space management
- Storage allocation
- Disk scheduling

### 6. Security and Protection
- Controlling access to system resources
- User authentication
- Data encryption and protection

---

## Types of Operating Systems

### 1. Batch Operating Systems
- Groups similar jobs and executes them in batches
- No direct interaction with the user
- Examples: IBM's OS/360, older mainframe systems

### 2. Time-Sharing Operating Systems
- Multiple users can use the system simultaneously
- CPU time is divided among multiple users
- Examples: UNIX, Multics

### 3. Distributed Operating Systems
- Multiple independent computers appear as a single system
- Resources are shared across the network
- Examples: Amoeba, Plan 9

### 4. Real-Time Operating Systems (RTOS)
- Processes data as it comes in, typically without buffering delays
- Used in time-critical applications
- Examples: VxWorks, QNX, RTLinux

### 5. Network Operating Systems
- Provides networking capabilities
- Manages data, users, groups, security, applications
- Examples: Windows Server, Linux Server

### 6. Mobile Operating Systems
- Designed for mobile devices like smartphones and tablets
- Examples: Android, iOS, HarmonyOS

---

## Process Management

### What is a Process?
A process is a program in execution. It includes:
- **Program Code** (text section)
- **Current Activity** (program counter, register contents)
- **Stack** (temporary data like function parameters, return addresses)
- **Data Section** (global variables)
- **Heap** (dynamically allocated memory)

### Process States
1. **New**: Process is being created
2. **Ready**: Process is waiting to be assigned to a processor
3. **Running**: Instructions are being executed
4. **Waiting**: Process is waiting for some event to occur
5. **Terminated**: Process has finished execution

### Process Control Block (PCB)
Contains information about the process:
- Process state
- Program counter
- CPU registers
- CPU scheduling information
- Memory management information
- Accounting information
- I/O status information

### Context Switching
The process of storing the state of a process so it can be restored later, and loading the state of another process.

---

## Memory Management

### Memory Management Techniques

#### 1. Contiguous Memory Allocation
- **Fixed Partitioning**: Memory divided into fixed-size partitions
- **Variable Partitioning**: Memory divided into variable-size partitions

#### 2. Paging
- Physical memory divided into fixed-size blocks (frames)
- Logical memory divided into same-size blocks (pages)
- Eliminates external fragmentation
- May cause internal fragmentation

#### 3. Segmentation
- Memory divided into segments of different sizes
- Each segment represents a logical unit (code, data, stack)
- More natural for programmers

#### 4. Virtual Memory
- Separation of logical memory from physical memory
- Only part of the program needs to be in memory for execution
- Allows execution of programs larger than physical memory

### Page Replacement Algorithms
- **FIFO (First-In-First-Out)**: Replace the oldest page
- **LRU (Least Recently Used)**: Replace the page not used for longest time
- **Optimal**: Replace page that won't be used for longest time (theoretical)
- **LFU (Least Frequently Used)**: Replace page with smallest count

---

## File System Management

### File System Structure
- **File**: Named collection of related information
- **Directory**: Container for files and other directories
- **File System**: Method of storing and organizing files

### File Operations
- Create
- Open
- Read
- Write
- Reposition (seek)
- Delete
- Truncate
- Close

### Directory Structure
- **Single-Level Directory**: All files in one directory
- **Two-Level Directory**: Separate directory for each user
- **Tree-Structured Directory**: Hierarchical structure
- **Acyclic-Graph Directory**: Allows sharing of subdirectories and files
- **General Graph Directory**: Allows cycles (requires garbage collection)

### File Allocation Methods
- **Contiguous Allocation**: Each file occupies contiguous blocks
- **Linked Allocation**: Each file is a linked list of disk blocks
- **Indexed Allocation**: Index block contains pointers to all blocks

---

## I/O Management

### I/O Hardware
- **Port**: Connection point for device
- **Bus**: Set of wires and protocol for message passing
- **Controller**: Electronics that operate the port, bus, or device

### I/O Methods
1. **Programmed I/O**: CPU continuously checks I/O device status
2. **Interrupt-Driven I/O**: Device notifies CPU when ready
3. **Direct Memory Access (DMA)**: Device controller transfers data directly to/from memory

### I/O Scheduling
- **FCFS (First-Come, First-Served)**: Process requests in order received
- **SSTF (Shortest Seek Time First)**: Select request with minimum seek time
- **SCAN**: Move in one direction, servicing requests, then reverse
- **C-SCAN**: Move in one direction, jump to beginning, repeat
- **LOOK/C-LOOK**: Like SCAN/C-SCAN but only go as far as last request

---

## Scheduling Algorithms

### CPU Scheduling Criteria
- **CPU Utilization**: Keep CPU as busy as possible
- **Throughput**: Number of processes completed per time unit
- **Turnaround Time**: Time from submission to completion
- **Waiting Time**: Time spent in ready queue
- **Response Time**: Time from submission to first response

### Scheduling Algorithms

#### 1. First-Come, First-Served (FCFS)
- Simplest scheduling algorithm
- Non-preemptive
- Can cause convoy effect

#### 2. Shortest Job First (SJF)
- Select process with smallest next CPU burst
- Can be preemptive or non-preemptive
- Optimal for minimum average waiting time
- Difficulty: knowing length of next CPU burst

#### 3. Priority Scheduling
- Each process assigned a priority
- CPU allocated to highest priority process
- Can be preemptive or non-preemptive
- Problem: Starvation (low priority processes may never execute)
- Solution: Aging (gradually increase priority)

#### 4. Round Robin (RR)
- Each process gets small unit of CPU time (quantum)
- Preemptive
- Fair allocation of CPU
- Performance depends on quantum size

#### 5. Multilevel Queue Scheduling
- Ready queue partitioned into separate queues
- Each queue has its own scheduling algorithm
- Fixed priority between queues

#### 6. Multilevel Feedback Queue
- Allows processes to move between queues
- Separates processes according to characteristics
- Most general and complex

---

## Deadlocks

### What is a Deadlock?
A situation where a set of processes are blocked because each process is holding a resource and waiting for another resource held by another process.

### Necessary Conditions for Deadlock
All four conditions must hold simultaneously:
1. **Mutual Exclusion**: At least one resource must be held in non-sharable mode
2. **Hold and Wait**: A process must be holding at least one resource and waiting for additional resources
3. **No Preemption**: Resources cannot be preempted
4. **Circular Wait**: A circular chain of processes exists, where each process waits for a resource held by the next process

### Handling Deadlocks

#### 1. Deadlock Prevention
Ensure at least one of the necessary conditions cannot hold
- Eliminate mutual exclusion
- Eliminate hold and wait
- Allow preemption
- Eliminate circular wait

#### 2. Deadlock Avoidance
System has additional information about resource requests
- **Banker's Algorithm**: Check if allocation leaves system in safe state
- **Resource Allocation Graph**: Detect cycles

#### 3. Deadlock Detection and Recovery
Allow deadlocks to occur, then detect and recover
- Detection: Maintain resource allocation graph
- Recovery: Process termination or resource preemption

#### 4. Deadlock Ignorance
Ignore the problem (used by most operating systems including UNIX)

---

## Synchronization

### Critical Section Problem
A section of code where shared resources are accessed. Only one process should execute in its critical section at a time.

### Requirements for Solution
1. **Mutual Exclusion**: Only one process in critical section
2. **Progress**: Decision on who enters critical section cannot be postponed indefinitely
3. **Bounded Waiting**: Limit on times other processes can enter critical section before a waiting process

### Synchronization Tools

#### 1. Locks
- **Mutex Lock**: Binary lock (locked/unlocked)
- **Spinlock**: Process spins while waiting

#### 2. Semaphores
- Integer variable accessed through two atomic operations: `wait()` and `signal()`
- **Binary Semaphore**: Can be 0 or 1
- **Counting Semaphore**: Can range over unrestricted domain

#### 3. Monitors
- High-level abstraction for process synchronization
- Only one process can be active within the monitor at a time
- Condition variables allow waiting and signaling

#### 4. Condition Variables
- Used with monitors
- Allow processes to wait for certain conditions
- Operations: `wait()` and `signal()`

### Classical Synchronization Problems

#### 1. Producer-Consumer Problem
- Producer produces data and puts it in a buffer
- Consumer removes data from the buffer
- Buffer has limited size

#### 2. Readers-Writers Problem
- Multiple processes want to read/write shared data
- Multiple readers can read simultaneously
- Only one writer can write at a time

#### 3. Dining Philosophers Problem
- Five philosophers sitting at a table
- Need two forks to eat
- Must avoid deadlock and starvation

---

## Additional Topics

### Threads
- Lightweight process
- Shares code, data, and resources with other threads of same process
- Has own program counter, register set, and stack
- Types: User-level threads, Kernel-level threads
- Models: Many-to-One, One-to-One, Many-to-Many

### Inter-Process Communication (IPC)
- **Shared Memory**: Processes share region of memory
- **Message Passing**: Processes communicate by exchanging messages
  - Direct/Indirect communication
  - Synchronous/Asynchronous communication
  - Buffering strategies

### Security and Protection
- **Authentication**: Verify user identity
- **Authorization**: Control access to resources
- **Access Control Lists (ACL)**: List of permissions attached to object
- **Capabilities**: Token giving rights to access object
- **Encryption**: Transform data to protect confidentiality

---

## Conclusion

Operating Systems form the foundation of modern computing, managing hardware resources and providing essential services for applications. Understanding these concepts is crucial for:
- System programming
- Software development
- Performance optimization
- Security implementation
- Distributed systems design

This documentation covers the fundamental concepts typically studied in a fourth-semester engineering curriculum. Practical lab work will reinforce these theoretical concepts through hands-on implementation and experimentation.

---

## References and Further Reading
- *Operating System Concepts* by Abraham Silberschatz, Peter B. Galvin, and Greg Gagne
- *Modern Operating Systems* by Andrew S. Tanenbaum
- *Operating Systems: Three Easy Pieces* by Remzi H. Arpaci-Dusseau and Andrea C. Arpaci-Dusseau
- Linux kernel documentation: https://www.kernel.org/doc/
- POSIX standards documentation

