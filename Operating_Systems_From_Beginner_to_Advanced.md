# Chapter 1 – A Dialogue on the Book  


## Why “Three Easy Pieces”?  
- The authors narrowed OS into **three central ideas (pieces):**
  1. **Virtualization**  
     - Abstracting and managing physical resources (e.g., CPU, memory, devices).  
     - Examples: virtual memory, CPU scheduling, virtual machines.  
  2. **Concurrency**  
     - Managing multiple tasks at the same time.  
     - Ensures safe and efficient sharing of resources among processes and threads.  
     - Core problems: synchronization, deadlocks, race conditions.  
  3. **Persistence**  
     - Ensuring data survives even after power loss or system crashes.  
     - Topics: file systems, storage devices, data consistency, recovery.  

---
✦━━━━━━━━━━━━━━━✦
   End of Chapter
✦━━━━━━━━━━━━━━━✦
---



# Chapter 2: Introduction to Operating Systems

This chapter introduces the fundamental concepts and functions of an operating system (OS). It highlights the core problems an OS solves and the key design goals that guide its development.

---

## What is an Operating System?

* An **operating system (OS)** is a body of software responsible for making a computer system easy to use.
* It manages **physical resources** like the processor (CPU), memory (DRAM), and storage (disk).
* The OS provides **abstractions** to make these resources more general, powerful, and user-friendly.

---

## The Core Functions of an OS

The OS performs three primary functions, often referred to as a **virtual machine**, **standard library**, and **resource manager**.

### 1. Virtualization

* The OS creates the **illusion** that each program has its own private, dedicated resources, even though they are shared.
* This process is called **virtualization**.
* **Virtualizing the CPU**: The OS makes a single CPU appear as if there are many **virtual CPUs**, allowing multiple programs to run seemingly at the same time. This is achieved by rapidly switching between programs.
* **Virtualizing Memory**: The OS gives each program its own **private virtual address space**, even though they share the same physical memory. This prevents one program from interfering with another's data.

### 2. Concurrency

* This refers to the challenges that arise when multiple tasks (like threads) run at the same time within the same program.
* A key problem is **race conditions**, where the final outcome depends on the unpredictable order of operations.
* The OS provides primitives and mechanisms to help programmers build **correct concurrent programs** by managing shared resources and ensuring **atomic** operations.

### 3. Persistence

* **Persistence** is the ability to store data reliably and long-term, even if the power goes off or the system crashes.
* The OS manages **input/output (I/O) devices** like hard drives and SSDs, which are used for persistent storage.
* The **file system** is the part of the OS responsible for managing files and directories, ensuring data is stored reliably and efficiently on these devices. Unlike virtualization, files are often shared between applications.
* The OS acts as a **standard library** by providing a set of system calls (APIs) for applications to interact with devices and other resources, abstracting away the complex, low-level details.

---

## Design Goals of an OS

* **Abstractions**: Providing a higher-level, simpler interface to complex hardware.
* **High Performance**: Minimizing the **overheads** (extra time and space) introduced by the OS to ensure efficient operation.
* **Protection**: Ensuring that one application's malicious or accidental behavior doesn't harm other applications or the OS itself. This is achieved through **isolation**.
* **Reliability**: Building a robust system that can handle failures and recover gracefully, as a system crash affects all running applications.

---

## Summary

The OS is a crucial piece of software that virtualizes resources, manages concurrency, and provides persistent storage. It achieves these goals while striving for high performance, reliability, and security. The core of an OS lies in finding the right **trade-offs** between these competing goals.

---
✦━━━━━━━━━━━━━━━✦
   End of Chapter
✦━━━━━━━━━━━━━━━✦
---

# Chapter 3 – A Dialogue on Virtualization

## 1. Introduction to Virtualization
- Virtualization is one of the **three central pieces** of Operating Systems (along with Concurrency and Persistence).
- Core idea: **Create the illusion of multiple independent resources from a single physical resource.**
- Analogy used: **The Peach Analogy**
  - **Physical peach** → the real resource (e.g., CPU).
  - **Virtual peaches** → illusions given to many users, making each think they have their own.
  - In reality, there’s still **only one physical peach**, but the illusion is convincing.

---
✦━━━━━━━━━━━━━━━✦
   End of Chapter
✦━━━━━━━━━━━━━━━✦
---

# Chapter 4: The Abstraction: The Process
In this chapter, we discuss one of the most fundamental abstractions that the OS provides to users: the **process**. The definition of a process, informally, is quite simple: it is a running program 
This chapter also explains how the OS creates the illusion of multiple CPUs. It discusses the components of a process, its life cycle, and the data structures used to manage it.

---

## The Abstraction: A Process

* A **process** is a running program. A program is a static set of instructions on disk, but a process is a dynamic, active instance of that program.
* The OS **virtualizes the CPU** through **time sharing**, switching between different processes to create the illusion that they are all running simultaneously on a single CPU.
* **Mechanisms** are low-level machinery that implement OS functionality (e.g., a **context switch**).Mechanisms are low-level methods or protocols that implement a needed piece of functionality.On top of these mechanisms resides some of the intelligence in the
OS, in the form of policies.
* **Policies** are high-level algorithms that make decisions (e.g., a **scheduling policy** decides which process to run next). A good OS design separates policy from mechanism.

### TIP: USE TIME SHARING (AND SPACE SHARING)
**Time sharing** is one of the most basic techniques used by an OS to share a resource. By allowing the resource to be used for a little while by one entity, and then a little while by another, and so forth, the resource in question (e.g., the CPU, or a network link) can be shared by many. The natural counterpart of time sharing is **space sharing**, where a resource is divided (in space) among those who wish to use it. For example, disk space is naturally a space-shared resource, as once a block is assigned to a file, it is not likely to be assigned to another file until the user deletes it.

### What Constitutes a Process?

A process is characterised by its **machine state**, which includes:

* **Memory (Address Space)**: This holds the program's code, static data, heap, and stack.
* **Registers**: These include general-purpose registers and special registers like the **program counter (PC)**, which points to the next instruction to execute, and the **stack pointer (SP)**.
* **Persistent Storage**: Information related to I/O, such as a list of currently open files.

---

## Process API

The OS provides an **Application Programming Interface (API)** for managing processes. Key functions include:

* **Create**: To load a program and create a new process instance.
* **Destroy**: To forcefully terminate a process.
* **Wait**: To pause and wait for another process to finish.
* **Miscellaneous Control**: To suspend or resume a process.
* **Status**: To get information about a process.

### Process Creation in Detail

The OS transforms a program into a process through these steps:
1.  **Loading**: The OS reads the program's code and static data from disk into the process's memory. This can be done eagerly (all at once) or lazily (as needed).
2.  **Stack Initialization**: A stack is created and initialized for local variables, function parameters, and return addresses. The OS also passes command-line arguments (argc, argv) to the main() function.
3.  **Heap Allocation**: A small heap is allocated for dynamic data. The heap can grow as the program requests more memory.
4.  **I/O Setup**: File descriptors for standard input, output, and error are set up.
5.  **Execution**: The OS jumps to the program's entry point (e.g., main()), transferring control to the new process.

---

## Process States

A process can be in one of three simplified states:
* **Running**: The process is currently executing instructions on the CPU.
* **Ready**: The process is ready to run but is waiting for the OS to schedule it.
* **Blocked**: The process is waiting for an event to complete (e.g., I/O operation) and is not ready to run.
* **Scheduled**: The OS moves a process from the **ready** state to the **running** state.
* **Descheduled**: The OS moves a process from the **running** state to the **ready** state.

---

## Data Structures

The OS uses a **process list** to keep track of all processes. Each entry in the list is often called a **Process Control Block (PCB)** and contains vital information about the process.

Key information tracked in a PCB:
* **Process State**: The current state of the process (e.g., Running, Ready, Blocked).
* **Process ID (PID)**: A unique identifier for the process.
* **Register Context**: Saved values of the process's registers, which are stored when the process is not running. This allows the OS to resume the process correctly later.
* **Other Information**: Parent process, open files, current working directory, etc.

Some systems also have additional states like **"Embryo"** (during creation) and **"Zombie"** (exited but not yet cleaned up by its parent process).

---
✦━━━━━━━━━━━━━━━✦
   End of Chapter
✦━━━━━━━━━━━━━━━✦
---

# Chapter 5: Interlude: Process API

This chapter focuses on the core UNIX system calls for managing processes: `fork()`, `exec()`, and `wait()`. These APIs are foundational to how modern shells and other programs create and control processes.

---

## 1. The `fork()` System Call

* The `fork()` system call creates a new process, known as the **child process**, as an almost identical copy of the calling process, known as the **parent process**.
* The parent and child processes are nearly identical, with their own private copies of the memory, registers, and other machine state.
* The only key difference is the value returned by `fork()`:
    * The **parent** receives the **Process ID (PID)** of the newly created child.
    * The **child** receives a return value of **0**.
* This design allows developers to write code that behaves differently in the parent and child processes.
* The output of a program that calls `fork()` is **non-deterministic** because the OS scheduler can run either the parent or the child first.

---

## 2. The `wait()` System Call

* The `wait()` system call is used by a parent process to **pause its execution** and wait for a child process to finish.
* When a child process exits, the `wait()` call in the parent returns.
* This system call can be used to make the execution flow **deterministic**, as the parent will not proceed until the child is complete.

---

## 3. The `exec()` System Call

* The `exec()` family of system calls (e.g., `execvp()`) loads a new program into the current process's memory space and begins executing it.
* A key point is that `exec()` **does not create a new process**. Instead, it **replaces** the current process's code, static data, heap, and stack with those of the new program.
* A successful call to `exec()` never returns, because the original program is overwritten.

---

## The `fork()` and `exec()` Combination

* The separation of `fork()` and `exec()` is a powerful design choice in UNIX.
* It allows a program, like a **shell**, to perform tasks **between** creating a child process and executing a new program.
* This intermediate stage is crucial for implementing features like **I/O redirection** and **pipes**.

### Example: I/O Redirection

* When you use a command like `wc p3.c > newfile.txt`, the shell works as follows:
    1.  The shell calls `fork()` to create a child process.
    2.  The child process closes its standard output file descriptor.
    3.  The child opens a new file, `newfile.txt`. The OS assigns this new file descriptor a low number, which often becomes the new standard output.
    4.  The child calls `exec()` to run the `wc` program.
    5.  The `wc` program, now running in the child process, writes its output to standard output, which is now redirected to `newfile.txt`.
    6.  The parent process calls `wait()` to wait for the child to finish.
* This same principle applies to **pipes** (`|`), which redirect the standard output of one process to the standard input of another via an in-kernel queue.

---

## Other Process APIs

* **`kill()`**: A system call used to send a signal to a process, such as to terminate or suspend it.
* **`ps` and `top`**: Command-line tools that provide information about the processes currently running on the system, their resource usage, and their state.

---
✦━━━━━━━━━━━━━━━✦
   End of Chapter
✦━━━━━━━━━━━━━━━✦
---

# Chapter 6: Mechanism: Limited Direct Execution

This chapter explains how the OS virtualizes the CPU using **Limited Direct Execution**, a technique that balances high performance with OS control.

---

## The Challenge of CPU Virtualization

To virtualize the CPU, the OS must share a single physical CPU among many processes. This requires solving two key problems:
* **Performance**: How to implement virtualization efficiently without adding excessive overhead.
* **Control**: How to ensure the OS retains control over the CPU to prevent a single process from taking over the machine or performing restricted operations.

The solution is to use a technique called **Limited Direct Execution**, which leverages hardware support.

---

## Limited Direct Execution: The Basic Idea

The core principle is to run the program **directly** on the CPU for high performance. The "limited" part of the name comes from the restrictions the OS places on the process to maintain control.


![Figure 6.1: Direct Execution Protocol (Without Limits)](./os_notes_images/6_1.png)

There are two main problems this model must solve:
### Problem 1: Restricted Operations

Certain operations, such as I/O or accessing privileged memory, must be restricted to prevent a process from harming the system.

* **User Mode vs. Kernel Mode**: The hardware supports two privilege levels:
    * **User Mode**: For applications, with restricted access to hardware.
    * **Kernel Mode**: For the OS (the kernel), with full, unrestricted access.
* **System Calls**: A user process must use a **system call** to request a privileged operation.
* **Trap Instruction**: A special hardware instruction that switches the CPU from user mode to kernel mode and jumps into a pre-defined OS handler.
* **Trap Table**: The OS sets this up at boot time to specify the addresses of its trap handlers. This ensures that a process can only jump to authorized entry points within the kernel.
* **Return-from-Trap**: A special instruction used by the OS to return to the user process, restoring its state and reverting to user mode.

---
![Figure 6.2: Limited Direct Execution Protocol](./os_notes_images/6_2.png)
### Problem 2: Regaining Control of the CPU

Since the OS isn't running when a user process is, it needs a way to regain control to perform tasks like switching between processes.

* **Cooperative Approach**: The OS relies on processes to voluntarily give up the CPU by making a system call or using an explicit `yield()` call. This is unreliable, as a buggy or malicious process could enter an infinite loop and never give up control.
* **Non-Cooperative Approach (Preemption)**: Modern OSes use a **timer interrupt**. The OS programs a hardware timer to generate an interrupt after a set period. When the interrupt occurs, the hardware automatically stops the running process, saves its state, and forces a jump into the OS's timer interrupt handler.

---

### Context Switching

Once the OS regains control, it can perform a **context switch** to a different process. This is a low-level operation involving:
1.  **Saving State**: The OS saves the state (e.g., register values, program counter) of the current process into its **Process Control Block (PCB)**.
2.  **Loading State**: The OS loads the state of the new process from its PCB.
3.  **Switching Stacks**: The OS switches to the kernel stack of the new process.
4.  **Returning to New Process**: The OS uses a `return-from-trap` instruction, causing the hardware to restore the new process's registers and resume its execution.

The combination of hardware support (modes, traps, timer interrupts) and OS software is the foundation of Limited Direct Execution, enabling efficient CPU virtualization and protection.
---
✦━━━━━━━━━━━━━━━✦
   End of Chapter
✦━━━━━━━━━━━━━━━✦
---
