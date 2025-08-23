# Operating System Concepts

**Author:** Abhinav P Pradeep 

## What is an Operating System (OS)?

**Definition**:
An Operating System is system software that acts as an interface between the user and the hardware.

**Think of OS as**:
A translator helping humans (users) talk to machines (hardware).

**Hardware Examples**:
In a computer:
- CPU (Central Processing Unit) – The brain
- Input Devices – Keyboard, Mouse, Scanner
- Output Devices – Printer, Monitor
- Main Memory – RAM
- Secondary Storage – Hard Disk (HDD/SSD)

You don’t interact with hardware directly. The OS handles this interaction on your behalf.

### Why Do We Need an OS?

**Without an OS**:
- You’d need to write separate programs to interact with each hardware device (printer, CPU, etc.).
- No resource control: One user may lock a device forever.
- Managing hardware manually is inefficient, insecure, and slow.

**With an OS**:
- Simplifies tasks (e.g., printing with Ctrl+P)
- Ensures resource sharing, security, and efficiency

### Primary Goals of an OS

| Goal                | Description                              |
|---------------------|------------------------------------------|
| Convenience         | Easy for users to access and manage hardware/software |
| Efficiency (Throughput) | Maximize number of tasks done per unit time |

### Real-Life OS Examples

| OS Name | Known For     | Market Share (then) |
|---------|---------------|---------------------|
| Windows | High convenience | ~95% earlier, ~82% now |
| Linux   | High throughput | Gaining popularity |
| macOS   | Security & UX | Apple ecosystem    |

### Functionalities of Operating System

#### Resource Management
Manages all hardware resources (CPU, memory, I/O).
- Especially important in multi-user environments (e.g., servers).
- Example: Task Manager shows CPU & RAM usage — managed by OS.

#### Process Management
OS handles execution of multiple processes simultaneously.
- Enables Multitasking / Multiprogramming.
- Uses CPU Scheduling Algorithms (like FCFS, Round Robin, etc.).
- Converts a program (.exe, .c) into a process for execution.

#### Storage Management
Manages secondary storage (HDD/SSD).
- Stores user files permanently.
- Uses File Systems like:
  - Windows: NTFS, FAT32
  - Linux: EXT4, NFS, CIFS
- Handles:
  - File creation/deletion
  - Disk space allocation

#### Memory Management
Manages RAM (volatile memory).
- RAM is limited.
- OS decides:
  - How much memory to give to a process
  - When to take it back
- Handles:
  - Swapping
  - Allocation & Deallocation
  - Segmentation & Paging

#### Security & Privacy
OS ensures only authorized users and processes access resources.
- Example: Login password — verified using Kerberos (in Windows).
- Memory Protection:
  - Each process is given its own segment in RAM
  - If a process tries to access another’s memory → Blocked

### OS in Action: Application Layer

**How users interact with OS**:
- Via Applications: MS Word, Media Player, Browsers
- Via Shell/Terminal:
  - Windows: Command Prompt
  - Linux: Terminal

These apps give commands → OS executes them via System Calls.

| Action       | System Call Invoked |
|--------------|---------------------|
| Open a file  | open()              |
| Print a document | write()         |
| Read file content | read()         |

### Summary Table

| Functionality          | Description                              |
|------------------------|------------------------------------------|
| Resource Management    | Allocates and deallocates CPU, memory, I/O, etc. |
| Process Management     | Handles multiple running processes efficiently |
| Storage Management     | Manages file storage on secondary devices |
| Memory Management      | Allocates RAM, handles swapping, protects segments |
| Security & Protection  | Prevents unauthorized access & ensures privacy |
| User Interface         | GUI or CLI to interact with OS           |

### Tips for Exam & Interviews
- OS acts as Manager, Mediator, and Protector
- Key terms: Multitasking, System Calls, Throughput, Swapping
- Examples like Ctrl+P, Command Prompt, Task Manager make answers relatable

## Batch Operating System

### What is a Batch Operating System?

A Batch Operating System is one in which similar jobs are grouped together (in a batch) and executed sequentially without user interaction.

**Key Idea**:
- Jobs are prepared offline → Grouped by an operator → Fed into the system as a batch

### Historical Context
- Era: 1960s
- Used by: Heavy computation organizations (e.g., NASA, ISRO)
- Input Mediums:
  - Punch cards
  - Paper tape / Magnetic tape
- User → Operator → Computer
- Users didn’t interact directly with the computer. They handed punch cards to operators.

### Working of Batch OS – Step by Step

```
User → Prepares job offline (punch card)
      ↓
Operator → Groups jobs into batches (B1, B2, ...)
      ↓
System → Feeds batch into CPU one by one
      ↓
CPU → Executes one job at a time sequentially
```

### Major Disadvantages

| Problem            | Why it Happened                          |
|--------------------|------------------------------------------|
| CPU Idle Time      | CPU waited during I/O — no parallel job handling |
| Non-preemptive Execution | Next job starts only after the current finishes |
| User has no control or interaction | Once submitted, user waits indefinitely |
| Delayed Results    | Jobs may take days, weeks, or months to complete |
| Heavy Dependence on Operators | Job submission & result collection had to go through them |

### Key Characteristics

| Feature         | Description                              |
|-----------------|------------------------------------------|
| Job Grouping    | Similar jobs grouped into batches        |
| Offline Job Submission | No real-time input from users       |
| No Interactivity | No command-line or GUI                  |
| Sequential Execution | Jobs executed one by one            |
| Operator Role   | Acts as a middleman between user & CPU   |

### Example: Real-life Analogy
Like submitting multiple answer sheets to an examiner. Examiner checks one sheet completely before moving to next. You can't edit or talk once submitted.

### Evolution: From Operators to Monitors
- Initial systems used operators to load and manage jobs.
- Later, monitors were introduced to automate job execution.
  - Allowed direct loading of jobs into the system.
- Notable early systems:
  - IBM’s FORTRAN Monitor System (FMS)
  - IBSYS on IBM 709X series

### Batch OS vs Modern OS (Quick Snapshot)

| Aspect             | Batch OS (1960s)                 | Modern OS (Today)                  |
|--------------------|----------------------------------|------------------------------------|
| User Interaction   | No direct interaction            | CLI / GUI available                |
| Job Execution      | Sequential (non-preemptive)      | Concurrent / Multitasking          |
| CPU Utilization    | Low (Idle during I/O)            | High (Context switching, scheduling) |
| Submission Mode    | Offline (punch cards)            | Real-time (Apps, shell, programs)  |
| Example Systems    | IBM FORTRAN Monitor, IBSYS       | Windows, Linux, macOS              |

### Exam-Ready Summary

| Point                  | Detail                                   |
|------------------------|------------------------------------------|
| Full Form              | Batch Operating System                   |
| Job Submission Method  | Offline (Punch Cards, Tapes)             |
| Execution Style        | Sequential, Non-interactive              |
| Major Disadvantage     | CPU Idle Time during I/O                 |
| Historical Systems     | FORTRAN Monitor System, IBM IBSYS        |
| Usage Time             | 1960s                                    |
| Interaction            | Through Operators (no user interaction with CPU) |

## Multiprogramming vs Multitasking

### Confusion Clarified
Many students confuse:

| Term            | Reality                          |
|-----------------|----------------------------------|
| Multiprogramming | Older concept (non-preemptive)   |
| Multitasking / Time-Sharing | Modern OS behavior (preemptive) |

### Multiprogramming OS

**Definition**: Multiple programs are kept in RAM, and the CPU executes one at a time (till I/O or end).

- Non-preemptive execution.

**Analogy**:
- 10 students have 5 questions each.
- CPU (Teacher) goes to Student 1 → Solves all 5 questions → Then to Student 2 → All 5 → Then Student 3 → All 5 … and so on.
- If Student 1 asks for calculator (I/O), teacher pauses & helps Student 2 next.

**How It Works**:
- Multiple processes in RAM
- Only 1 process gets CPU at a time
- If process requests I/O, CPU switches to another

**Goals**:

| Focus Area            | Description                      |
|-----------------------|----------------------------------|
| Maximize CPU Utilization | Avoid CPU idle when I/O happens  |
| Simple Scheduling     | No forceful switching – only if needed |

**Limitations**:
- Slow response time for later processes
- No real-time interactivity
- Users must wait long for response

### Multitasking (Time-Sharing) OS

**Definition**: OS gives each process a time slice of CPU (time quantum), and preempts to next if time is up.

- Preemptive scheduling

**Analogy**:
- Same 10 students.
- CPU decides: I’ll solve only 2 questions per student at a time → Goes to Student 1 → Q1, Q2 → Then Student 2 → Q1, Q2 → ... → Student 10 → Q1, Q2 → Then comes back again for Q3, Q4…
- Everyone gets attention early

**How It Works**:
- CPU switches rapidly between processes
- Uses scheduling algorithms (e.g., Round Robin)
- Gives illusion of parallel execution

**Goals**:

| Focus Area       | Description                      |
|------------------|----------------------------------|
| Responsiveness   | Reduce waiting time for users    |
| Fairness         | Every process gets chance to execute |
| Low Latency      | Quick response to user interactions |

### Multiprogramming vs Multitasking – Quick Comparison

| Feature              | Multiprogramming                 | Multitasking / Time-Sharing      |
|----------------------|----------------------------------|----------------------------------|
| Execution Type       | Non-preemptive                   | Preemptive                       |
| CPU Allocation       | Until completion or I/O request  | For fixed time quantum           |
| Main Goal            | Maximize CPU usage               | Minimize response time           |
| Responsiveness       | Low                              | High                             |
| Example Analogy      | 1 student full work → then next  | All students get partial attention |
| Real-World Use Today | Rare (historical)                | Common (used in laptops/phones)  |
| Scheduling Algorithms | Simple / Static                  | Round Robin, Priority Scheduling |
| CPU Idle             | Reduced but still possible       | Almost none                      |

### Where Are They Used?

| OS Type          | Real Example                     |
|------------------|----------------------------------|
| Multiprogramming | Historical mainframes (e.g. IBM 7094) |
| Multitasking     | Windows, macOS, Linux, Android, iOS |

### Key Exam Points

| Concept                  | Remember                                 |
|--------------------------|------------------------------------------|
| Fork creates new process | Uses system call, returns child with PID = 0 |
| Threads share memory     | Fast, efficient, but blocking is dangerous |
| Context switch speed     | Thread > Process                         |
| Independence             | Process = independent, Thread = interdependent |
| Blocking                 | Blocking one process does NOT block others |
|                          | Blocking one thread blocks the entire process |

**Final Verdict**:

| Use Process When...            | Use Thread When...               |
|--------------------------------|----------------------------------|
| You need isolation or security | You need speed and shared memory |
| You expect crash resilience    | You expect high parallelism and low overhead |

## Types of Operating Systems

### Real-Time, Distributed, Clustered, Embedded

| OS Type          | Core Idea                                 | Typical Place                       | Must-Remember Keywords            |
|------------------|-------------------------------------------|-------------------------------------|-----------------------------------|
| Real-Time (RTOS) | Meet deadlines every time                 | Missile control, Pacemakers, Live stock trading | Deadline, Deterministic, Hard vs Soft |
| Distributed      | Many geographically-spread computers cooperate via a network | Google search, Netflix CDN, Blockchain | Loose coupling, Location transparency, Fault tolerance |
| Clustered        | Several machines on one local network act as a single powerful server | Supercomputers, High-performance DB clusters | Tightly coupled, Load balancing, High availability |
| Embedded         | OS baked into a device with one dedicated job | Washing machine, Router, Car ECU | Resource-constrained, Firmware, Fixed function |

#### Real-Time Operating System (RTOS)

| Aspect          | Hard RTOS                              | Soft RTOS                              |
|-----------------|----------------------------------------|----------------------------------------|
| Deadline        | Must never slip                        | Occasional slip acceptable             |
| Example         | Air-bag controller, Missile guidance   | Online gaming, Live video stream       |
| Scheduling      | Rate-Monotonic, EDF                    | Hybrid priority + time slicing         |
| Failure Impact  | Catastrophic                           | Degraded QoS                           |

**Key Line**: “Correct result at the correct time, not just the correct result.”

#### Distributed Operating System
- “One system, many sites.”
- Computers are loosely coupled but behave (to the user) like one virtual machine.
- **Why Use It?**:
  - Scalability: Add servers as demand grows.
  - Fault tolerance: If node A dies, node B steps in.
  - Data locality: Serve users from the nearest region (CDN).
- **Hallmarks**:
  - Network OS layer handles message passing & transparency.
  - Uses concepts like distributed file systems (DFS) & consensus (Raft, Paxos).

#### Clustered Operating System
- “Many PCs, one rack, one purpose.”

| Feature        | Distributed                            | Clustered                              |
|----------------|----------------------------------------|----------------------------------------|
| Distance       | Often world-wide                       | Same room / rack                       |
| Coupling       | Loose                                  | Tighter (shared storage / heartbeat)   |
| Goal           | Availability + geo-scaling             | Raw performance, fail-over             |
| Example        | Google global infra                    | 64-node HPC cluster, Hadoop rack       |

**Building Blocks**:
- Node OS + Cluster middleware (e.g., Kubernetes, Hadoop YARN, Windows Failover Cluster).
- Heartbeat monitors node health; load balancer spreads work.

#### Embedded Operating System
- “Software inside stuff.”
- Signature traits:
  - ROM / Flash storage, tiny RAM (KB–MB).
  - No user install/uninstall; firmware updated only by vendor.
  - Real-time flavoured if timing is critical (e.g., ABS brakes).
- Micro-kernels & RTOS variants:
  - FreeRTOS, VxWorks, Zephyr, Embedded Linux (BusyBox)

### Cheat-Sheet Table

| Feature ➡      | Real-Time                              | Distributed                            | Clustered                              | Embedded                               |
|----------------|----------------------------------------|----------------------------------------|----------------------------------------|----------------------------------------|
| Primary Goal   | Timeliness                             | Geo-scalable services                  | High throughput & HA                   | Dedicated device control               |
| Coupling       | –                                      | Loose                                  | Tight                                  | –                                      |
| Failure Handling | Hard → system halt                     | Shift work to peers                    | Fail-over node                         | Often watchdog reset                   |
| User Interaction | Often none                             | Via network                            | Via network / API                      | None or minimal UI                     |
| Key Metric     | Deadline meet %                        | Availability                           | Speed + uptime                         | Power + cost                           |

## System Call

### What is a System Call?

A System Call is a programmatic interface that allows a user-level program to request services from the Operating System’s kernel, typically for resource access or privileged operations.

### Why System Calls?
- User programs run in User Mode (no direct hardware access).
- OS Kernel runs in Kernel Mode and controls hardware, memory, files, I/O.
- To perform tasks like file operations, memory access, process control, or hardware use, user mode must switch to kernel mode using a System Call.

**Example**:

```c
printf("Hello World");
```

- printf() is a library function, not a system call.
- Internally, it uses a write() system call to print on monitor (device).

### Mode Transition

| Action                  | Mode                          |
|-------------------------|-------------------------------|
| Execute user code (e.g., 2+2) | User Mode                 |
| Access file, I/O, hardware | Kernel Mode via System Call  |

System Call acts as a bridge between user-level program and kernel.

### Real Examples
- C / Linux: You can directly use open(), read(), write() system calls.
- Windows: 600–700 system calls available (e.g., via WinAPI).

### Categories of System Calls

| Category              | Description                              | Examples                     |
|-----------------------|------------------------------------------|------------------------------|
| File Manipulation     | Create, read, write, delete, open or close files. | open(), read(), write(), close(), unlink() |
| Device Manipulation   | Interact with I/O devices.               | ioctl(), read(), write(), lseek() |
| Process Control       | Create/terminate processes, manage execution. | fork(), exec(), exit(), wait(), kill() |
| Information Maintenance | Get system or process information.     | getpid(), getppid(), uname(), time() |
| Communication         | Inter-Process Communication (IPC).      | pipe(), shmget(), msgsnd(), connect() |
| Protection & Security | Set permissions, access control.       | chmod(), umask(), setuid()  |

### Common Linux System Calls

| Function            | System Call  |
|---------------------|--------------|
| Print to screen     | write()      |
| Read keyboard input | read()       |
| Open file           | open()       |
| Create new process  | fork()       |
| Replace process image | exec()     |
| Wait for child      | wait()       |
| Get process ID      | getpid()     |
| Change permissions  | chmod()      |
| Set alarms/timers   | alarm(), sleep() |

### System Call Flow (Behind the Scenes)

```
User Code (e.g., printf) 
   ↓
Library Function (e.g., stdio.h)
   ↓
System Call (e.g., write)
   ↓
Kernel Mode — Performs device-level operation
```

**Real-Life Analogy**:
- System Call is like ringing a bell at the manager’s office (kernel) to approve special requests (e.g., print, read file). You (user) can't do it directly.

## Fork System Call

### What is fork()?

fork() is a system call used in UNIX/Linux OS to create a new process.
- The new process is called the child process.
- The process that calls fork() is the parent process.

**Child is a clone of the parent**:
- It has its own Process ID (PID).
- It gets a copy of the parent’s code, stack, heap, and data segment.
- But both parent and child run independently and concurrently.

**Syntax (C)**:
```c
pid_t pid = fork();
```

### Return Values of fork()

| Return Value | Meaning                              |
|--------------|--------------------------------------|
| 0            | Returned in the child process        |
| > 0          | Positive value (PID of child) returned in the parent process |
| -1           | Error in creating child (rare, ignore for theory-based questions) |

**Key Concept**:
- After a single fork(), two processes start running: one parent and one child.

**Example**:
```c
main() {
  fork();
  printf("Hello\n");
}
```

**Output**:
```
Hello
Hello
```

Because both parent and child execute printf().

### Multiple fork() Calls

**Number of total processes created**:
- If there are n fork() calls:
  - Total number of processes = 2^n
  - Number of child processes = 2^n - 1

**Example: 2 forks**:
```c
main() {
  fork(); // 1st fork
  fork(); // 2nd fork
  printf("Hello\n");
}
```

- Total processes = 2^2 = 4
- Total child processes = 2^2 - 1 = 3
- Output: Hello printed 4 times

**Process Tree**:

```
Parent
├── Child1
│   └── Child3
└── Child2
```

**Example: 3 forks**:
```c
main() {
  fork();
  fork();
  fork();
  printf("Hello\n");
}
```

- Total processes = 2^3 = 8
- Child processes = 2^3 - 1 = 7
- Output: Hello printed 8 times

**Formula**:
- Total "Hello"s = 2^n
- Total Child Processes = 2^n - 1

**Notes**:
- fork() copies the process context, but both child and parent continue from the same point after the fork() call.
- Execution order is not deterministic (depends on scheduler).
- In questions, always assume ideal case — fork always succeeds (ignore -1).

### fork vs Thread (Quick Mention)

| Feature       | fork()                               | Thread                               |
|---------------|--------------------------------------|--------------------------------------|
| Process ID    | New                                  | Shared                               |
| Memory        | Separate                             | Shared                               |
| Overhead      | High                                 | Low                                  |
| Use           | Full new process                     | Lightweight parallel task            |

**Example Use Case**:
- Running different applications — e.g. browser, text editor (Process)
- Performing parallel tasks in same application — e.g. multiple tabs in a browser (Thread)

## Process vs Thread

| Aspect                  | Process                                  | Thread                                   |
|-------------------------|------------------------------------------|------------------------------------------|
| Definition              | Independent program in execution, heavyweight | Lightweight unit of a process, part of a process |
| Creation                | Via system call like fork() (Kernel-mode involvement) | Created by application/program (User-mode, fast) |
| Overhead                | High — complete duplicate of code, data, stack, registers | Low — threads share code and data, but each has own stack and registers |
| System Call             | Yes (e.g. fork(), requires kernel access) | Not needed for user-level threads        |
| Managed By              | Operating System                         | Application/User-level thread library (unless kernel-level threads) |
| Memory                  | Each process has separate memory space   | Threads of a process share memory space  |
| Context Switching       | Slower — requires saving full PCB, flushing caches | Faster — fewer values to save/restore (registers, stack pointer) |
| Communication (IPC)     | Needs Inter Process Communication (e.g. pipes, sockets) | Easier communication via shared memory   |
| Independence            | Processes are independent — blocking one doesn't block others | Threads are interdependent — blocking one blocks all in that process |
| Blocking                | Blocking one process does NOT block others | Blocking one thread blocks the entire process |
| Example Use Case        | Running different applications — e.g. browser, text editor | Performing parallel tasks in same application — e.g. multiple tabs in a browser |
| Identification          | Each process has its own PID (Process ID) | All threads share same PID, but have Thread IDs (TID) |
| Resource Sharing        | No (separate memory, stack, etc.)        | Yes (code, data, files shared)          |
| Typical Usage           | Used when tasks are independent and need resource isolation | Used when tasks are similar or related, e.g. multi-client server threads |

### Real-life Analogy
- Process = Human clone → Complete separate body, brain, identity
- Thread = Extra arm of same person → Same brain, same body, just extra workers

### Example Scenario
**Print Server**:
- Using processes → Each request spawns a full child process = slow and memory intensive.
- Using threads → One server process spawns lightweight threads for each print request = efficient, fast.

### Key Exam Points

| Concept                  | Remember                                 |
|--------------------------|------------------------------------------|
| Fork creates new process | Uses system call, returns child with PID = 0 |
| Threads share memory     | Fast, efficient, but blocking is dangerous |
| Context switch speed     | Thread > Process                         |
| Independence             | Process = independent, Thread = interdependent |
| Blocking                 | Blocking one process does NOT block others |
|                          | Blocking one thread blocks the entire process |

**Final Verdict**:

| Use Process When...            | Use Thread When...               |
|--------------------------------|----------------------------------|
| You need isolation or security | You need speed and shared memory |
| You expect crash resilience    | You expect high parallelism and low overhead |

### Threads

A thread is a lightweight process and forms the basic unit of CPU utilization. A process can perform more than one task at the same time by including multiple threads.
- A thread has its own program counter, register set, and stack
- A thread shares resources with other threads of the same process the code section, the data section, files and signals.

A new thread, or a child process of a given process, can be introduced by using the fork() system call.
A process with n fork() system calls generates 2^n - 1 child processes.

There are two types of threads:
- User threads
- Kernel threads

**Example**: Java thread, POSIX threads. **Example**: Window Solaris.

### Types of Threads

#### Based on the Number of Threads
There are two types of threads:
(i) Single thread process
(ii) Multi thread process

#### Based on Level
There are two types of threads:
(i) User-level threads
(ii) Kernel-level threads

### Process

A process is a program under execution. The value of program counter (PC) indicates the address of the next instruction of the process being executed. Each process is represented by a Process Control Block (PCB).

**Process States Diagram**:

[Diagram: New → Ready → Running → Waiting → Terminated, with transitions]

### Schedulers

The operating system deploys three kinds of schedulers:
- **Long-term scheduler**: It is responsible for the creation and bringing of new processes into the main memory (New → Ready state transition).
- **Medium-term scheduler**: The medium-term scheduler selects a process from the ready state to be executed (Ready → Run state transition).
- **Short-term scheduler**: The short-term scheduler selects a process from the ready state to be executed (Ready → Run state transition).

### Dispatchers

The dispatcher is responsible for loading the job (selected by the short-term scheduler) onto the CPU. It performs context switching. Context switching refers to saving the context of the process which was being executed by the CPU and loading the context of the new process that is being scheduled to be executed by the CPU.