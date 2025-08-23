# CPU Scheduling and Process Synchronization

**Author:** Abhinav P Pradeep 

## Introduction to CPU Scheduling

CPU Scheduling is the process by which the Operating System (OS) selects a process from the ready queue and allocates the CPU to it.

- In uniprocessor systems, only one process can execute at a time.
- Other processes remain in the ready queue (in RAM).
- Scheduling improves:
  - CPU utilization
  - Throughput
  - Turnaround time
  - Waiting time
  - Response time

## Classification of Scheduling

### Based on Preemption

| Type          | Description                                           |
|---------------|-------------------------------------------------------|
| Non-Preemptive | Once a process starts execution, it runs to completion. No interruption. |
| Preemptive    | A running process can be interrupted and put back in the ready queue. |

## Key Terms in Scheduling

| Term            | Description                              | Formula                 |
|-----------------|------------------------------------------|-------------------------|
| Arrival Time (AT) | Time when process enters ready queue     | –                       |
| Burst Time (BT) | CPU time required by process             | –                       |
| Completion Time (CT) | Time when process finishes execution | –                       |
| Turnaround Time (TAT) | CT - AT                             | TAT = CT - AT           |
| Waiting Time (WT) | TAT - BT                               | WT = TAT - BT           |
| Response Time (RT) | First CPU allocation - AT             | RT = First Start - AT   |

## Non-Preemptive Scheduling Algorithms

### First Come First Serve (FCFS)
- **Strategy**: Process with earliest arrival gets CPU first.
- **Type**: Non-preemptive
- **Example**: Queue at a ticket counter

**Pros**:
- Simple to implement
- Fair (in order of arrival)

**Cons**:
- Convoy Effect: Short processes stuck behind long ones
- Not optimal for average Waiting Time (WT)/Turnaround Time (TAT)

### Shortest Job First (SJF)
- **Strategy**: Process with shortest burst time executed first
- **Type**: Non-preemptive
- **Optimal**: Gives lowest average waiting time
- **Problem**: Requires knowledge of burst time in advance

**Example**:
| P1: 8ms | P2: 4ms | P3: 2ms |
Execute P3 → P2 → P1

**Pros**:
- Optimal WT and TAT

**Cons**:
- Hard to predict burst time
- Starvation of long processes

### Priority Scheduling
- Each process assigned a priority
- Lower number = higher priority
- Non-preemptive version: Once a process starts, it runs fully
- **Problem**: Starvation of lower priority processes
- **Solution**: Aging – Gradually increase priority of waiting processes

### Longest Job First (LJF)
- Reverse of SJF: Process with longest burst time gets CPU
- Non-preemptive
- Rarely used due to unfairness to short processes

### Highest Response Ratio Next (HRRN)
- **Formula**:
  ```
  Response Ratio = (Waiting Time + Burst Time) / Burst Time
                 = 1 + (WT / BT)
  ```
- The process with the highest ratio gets CPU
- Solves starvation in SJF

## Preemptive Scheduling Algorithms

### Round Robin (RR)
- Time-sharing system
- Each process gets CPU for fixed time quantum (q)
- After time expires → moved to back of queue

**Example**:
10 processes, each gets 2s → fairness improves

**Pros**:
- Good responsiveness
- Fair to all

**Cons**:
- Context switching overhead
- Poor for long processes

### Shortest Remaining Time First (SRTF)
- Preemptive version of SJF
- At any time, schedule the process with least remaining burst time

**Example**:
If P1 (BT=8ms) is running, and P2 (BT=2ms) arrives → preempt P1 and run P2.

**Pros**:
- Lowest average waiting time

**Cons**:
- Starvation
- Requires future burst time knowledge

### Preemptive Priority Scheduling
- Interrupts current process if a higher-priority process arrives
- **Cons**:
  - Starvation
  - Use Aging to resolve

### Longest Remaining Time First (LRTF)
- Reverse of SRTF
- Schedule the process with the maximum remaining time
- Rare in real systems

### Useful Facts About Scheduling Algorithms
1. FCFS can cause long waiting times, especially when the first job takes too much CPU time.
2. Both SJF and Shortest Remaining Time First algorithms may cause starvation, especially when shorter processes keep arriving.
3. If the time quantum for Round Robin scheduling is very large, it behaves the same as FCFS scheduling.
4. SJF is optimal in terms of average waiting time for a given set of processes, but the challenge is predicting the burst time of the next job.

## Problem-Solving Checklist
1. Draw Gantt Chart
2. Track Arrival and Completion Time
3. Apply formulas:
   - TAT = CT - AT
   - WT = TAT - BT
   - RT = First Start - AT
4. Average the values if needed

## Process Synchronization

Process Synchronization is the technique to ensure that multiple processes can execute concurrently without interfering with each other or leading to inconsistent data.

- Needed in multiprogramming/parallel systems.
- Ensures data consistency, deadlock avoidance, and process coordination.

### Serial vs Parallel Execution

| Execution Type | Description                              | Example                     |
|----------------|------------------------------------------|-----------------------------|
| Serial         | One process runs at a time. No overlap.  | ATM: One user at a time     |
| Parallel       | Multiple processes may execute simultaneously. | Online banking, IRCTC |

### Types of Processes

#### Independent Processes
- No shared data/code/resource
- No effect on each other
- Example: SBI and HDFC online transactions

#### Cooperative Processes
- Share variables, memory, resources, or code
- Execution of one affects the other
- Example: IRCTC bookings, shared printer, shared counter in multithreaded apps

### Why is Synchronization Needed?
Cooperative processes can lead to problems like:
- Race Conditions
- Inconsistent Data
- Deadlocks

**Analogy**: Guitar strings must be tuned (synchronized) to make music. Otherwise, they create noise.

### Critical Section Problem
1. **Critical Section**: The portion of the code in the program where shared variables are accessed and/or updated.
2. **Remainder Section**: The remaining portion of the program excluding the Critical Section.
3. **Race around Condition**: The final output of the code depends on the order in which the variables are accessed.

### Race Condition – Illustrated

**Scenario**:
- Two processes: P1 and P2
- Shared Variable: `shared = 5`

**P1 Code**:
```c
x = shared;
x++;
sleep(1);
shared = x;
```

**P2 Code**:
```c
y = shared;
y--;
sleep(1);
shared = y;
```

**Execution Flow with Uni-Processor**:
- Initially, `shared = 5`
1. P1 gets CPU:
   - `x = 5`
   - `x++` → `x = 6`
   - `sleep(1)` → context switch to P2
2. P2 executes:
   - `y = 5`
   - `y--` → `y = 4`
   - `sleep(1)` → back to P1
3. P1 resumes:
   - `shared = x` (6)
4. P2 resumes:
   - `shared = y` (4)

**Result**: Final `shared = 4` (which is WRONG)

**Expected Output**: `shared = 5` (increment and decrement cancel each other)

**Root Cause**: Shared variable access without synchronization

**Possible Outcomes**:

| First Process | Final Value of shared |
|---------------|-----------------------|
| P1            | 4 (wrong)             |
| P2            | 6 (wrong)             |

The final value depends on execution order, which is unpredictable. This unpredictability is called a Race Condition.

### Solution: Synchronization Techniques
To avoid race conditions:
- Use Mutual Exclusion
- Synchronize access to shared resources

**Techniques**:
- Locks / Mutexes
- Semaphores
- Peterson's Solution (software solution)
- Monitors (used in Java)

## Critical Section

### What is a Critical Section?
A critical section is a part of a program where shared resources (e.g., memory, variables, files) are accessed.

- Important in concurrent cooperative processes—multiple processes sharing something like memory or a variable.

**Example**:
```c
// Shared variable
int count = 0;
void increment() {
    // Critical Section
    count++;
}
```

### Code Division
In any concurrent program, the code is divided into:

| Section            | Description                              |
|--------------------|------------------------------------------|
| Critical Section   | Code accessing shared/common resources.  |
| Non-Critical Section | Code accessing private/independent resources. |

### Why Critical Sections Need Care?
If two processes access shared data at the same time, it can cause a Race Condition.

**Race Condition Example**:
```c
// P1: count++
temp = count;       // read
temp = temp + 1;
count = temp;       // write
// P2: count--
temp = count;       // read
temp = temp - 1;
count = temp;       // write
```

If both execute simultaneously, the final value of `count` is unpredictable!

### Solution: Synchronization
To avoid race conditions:
1. **Entry Section**: Code that checks if a process can enter the critical section.
2. **Exit Section**: Code that runs after leaving the critical section.
3. **Remainder Section**: Non-critical section code.

Only one process should enter the critical section at a time, enforced by synchronization mechanisms like Semaphores, Mutexes, etc.

### Conditions for a Good Synchronization Solution
A good solution must follow 4 conditions (2 primary, 2 secondary):

| #   | Condition                | Type       | Meaning                              |
|-----|--------------------------|------------|--------------------------------------|
| 1   | Mutual Exclusion         | Primary    | Only one process is allowed inside critical section at a time. |
| 2   | Progress                 | Primary    | If no one is in CS, a process interested should get a chance without being blocked by an idle one. |
| 3   | Bounded Waiting          | Secondary  | A process should not wait forever to enter CS. There should be a limit on waiting time. |
| 4   | No Hardware Assumptions  | Secondary  | Solution must be portable, not dependent on system speed, CPU architecture, or OS. |

#### Mutual Exclusion (Example)
If P1 is inside the bathroom (CS), P2 must wait outside. Think of a washroom lock: If it's locked, the other person can’t enter.

#### Progress (Example)
If no one is in the CS, and P1 wants to enter, but P2's code stops it, then progress is violated. No process should stop others unnecessarily if it itself doesn’t want to enter CS.

#### Bounded Waiting (Example)
- P1 entered CS 3 times, P2 is still waiting → Okay.
- P1 enters infinite times, P2 never enters → Starvation, violates bounded waiting.
- We must ensure that every process gets a chance eventually.

#### No Assumptions (Example)
- **Wrong approach**: “This solution works only on 32-bit Linux with 1GHz CPU.”
- **Correct approach**: Solution should run on any OS/hardware, i.e., should be universal and portable.

### Real-Life Analogy

| Concept               | Analogy                              |
|-----------------------|--------------------------------------|
| Critical Section      | Bathroom                             |
| Entry Section         | Door Lock                            |
| Mutual Exclusion      | Only one person at a time            |
| Bounded Wait          | Everyone gets a turn                 |
| Progress              | Don’t block if you’re not interested |
| No Assumptions        | Any house guest can use the bathroom |

## Semaphore

### What is a Semaphore?
A semaphore is a tool to prevent race conditions in concurrent/cooperative process environments. It ensures synchronization between processes.

**Defined as**:
“An integer variable used in a mutual exclusive manner by various concurrent cooperative processes to achieve synchronization.”

### Types of Semaphores

| Type       | Range / Values | Use Case                     |
|------------|----------------|------------------------------|
| Counting   | -∞ to +∞       | Track multiple resources     |
| Binary     | 0 or 1 (like a flag) | Acts like a lock mechanism |

### Structure: Process Synchronization
Each process follows this structure:
1. **Entry Section** – Code before entering critical section
2. **Critical Section** – Shared code or resource
3. **Exit Section** – Code after finishing in critical section

### Semaphore Operations

| Entry Code | Exit Code                     | Other Names                     |
|------------|-------------------------------|---------------------------------|
| P()        | V()                           |                                 |
| Down()     | Up()                          |                                 |
| Wait()     | Signal()                      | Post() / Release()              |

All are synonyms:
- P = Down = Wait
- V = Up = Signal = Post

#### How P() (Down / Wait) Works
```c
P(S):            // Entry code
S = S - 1;
if (S < 0)
    block the process (add to suspended list);
```

- If S ≥ 0 → process enters
- If S < 0 → process is blocked (goes to suspended list or sleep)

**Example**:
- If S = 3 → 3 processes can enter successfully.
- After 3 P() ops → S = 0
- Next P() → S becomes -1 → process is blocked

#### How V() (Up / Signal) Works
```c
V(S):            // Exit code
S = S + 1;
if (S ≤ 0)
    wake up one process from suspended list;
```

- Process is removed from blocked list
- Typically follows FIFO order for unblocking

### Complete Example Flow

| Step       | S Value | Action | Result                              |
|------------|---------|--------|-------------------------------------|
| Init       | 3       |        |                                     |
| P1 enters  | 2       | P()    | Enters                              |
| P2 enters  | 1       | P()    | Enters                              |
| P3 enters  | 0       | P()    | Enters                              |
| P4 tries   | -1      | P()    | Blocked (in suspended list)         |
| P5 tries   | -2      | P()    | Blocked                             |
| P1 exits   | -1      | V()    | S=-1, wake up P4 (now ready)        |
| P2 exits   | 0       | V()    | S=0, wake up P5 (now ready)         |
| P3 exits   | 1       | V()    | No wakeup (S > 0)                   |

### GATE-style Questions
**Q1**: If S = 10, how many processes can enter?
- **Ans**: 10 (each P() reduces S by 1)

**Q2**: If S = 10, and we do: 6 P(), 4 V()
- 6 P() → S = 4
- 4 V() → S = 8
- **Final S = 8**

**Q3**: S = 17, ops = 5P, 3V, 1P
- 17 - 5 + 3 - 1 = 14
- **Final S = 14**

### Key Takeaways
- Semaphore avoids race condition and ensures process synchronization
- Use P() to enter, V() to exit
- If S < 0, process is blocked
- FIFO order used to wake blocked processes
- Binary semaphore → for mutual exclusion
- Counting semaphore → for resource counting

## Binary Semaphore

### What is a Binary Semaphore?
A synchronization tool used between cooperative processes, same as Counting Semaphore but values are only 0 or 1.

**Defined as**:
“A semaphore that can hold only two values: 0 and 1.”

### Binary vs Counting Semaphore

| Feature        | Binary Semaphore | Counting Semaphore |
|----------------|------------------|--------------------|
| Values         | 0 or 1           | -∞ to +∞           |
| Complexity     | Simple           | More general       |
| Used for       | Mutual exclusion | Resource counting  |
| Preferred in practice | Yes         | Rare               |

### Operations Used

| Operation | Synonyms                   | Purpose                              |
|-----------|----------------------------|--------------------------------------|
| Down()    | P(), Wait()                | Entry code → check & block           |
| Up()      | V(), Signal(), Post()      | Exit code → release & wake up        |

#### How Down() Works (Binary)
```c
Down(S):
if (S == 1)
    S = 0;              // Successful
else
    block the process;  // S == 0 → Block it (unsuccessful)
```

- S = 1: Enter critical section (mutual exclusion maintained)
- S = 0: Cannot enter → process is blocked/suspended

#### How Up() Works (Binary)
```c
Up(S):
if (suspended list is empty)
    S = 1;              // Make critical section available
else
    wake up one process from suspended list;
```

- If no process is blocked → simply set S = 1
- If any process is blocked → wake one up and move to ready queue

### Example Execution
**Scenario**:

| Process | Operations                   |
|---------|------------------------------|
| P1      | Down(S) → Critical → Up(S)   |
| P2      | Down(S) → Critical → Up(S)   |

**Case 1**: S = 1 initially
- P2 calls Down(S) → S becomes 0, enters critical section
- P1 calls Down(S) → blocked (since S is 0)
- P2 finishes, calls Up(S) → wakes P1 (from suspended list)
- **Result**: Mutual exclusion maintained

**Case 2**: S = 0 initially
- Both P1 and P2 try Down(S)
- Both fail, both get blocked
- **Result**: Deadlock if no one ever gets unblocked

### Important Observations
- Binary semaphore ensures one process at a time in critical section.
- You don’t need to memorize code for exams – just remember the behavior of:
  - Down() → blocks if S == 0
  - Up() → wakes if blocked processes exist
- In competitive exams (e.g., GATE), initial S value is always given in the question

### Summary Cheat Sheet

| State                     | Operation | Result                              |
|---------------------------|-----------|-------------------------------------|
| S = 1                     | Down()    | S → 0, process enters               |
| S = 0                     | Down()    | Block the process                   |
| S = 0 + blocked procs     | Up()      | Wake up one process                 |
| S = 0 + no blocked procs  | Up()      | S → 1                               |
| S = 1 + no blocked procs  | Up()      | S stays 1                           |

## Mutex

### What is a Mutex?
Mutex stands for Mutual Exclusion. It is a lock-based synchronization tool used to ensure that only one process/thread can access a critical section at a time.

- **Think of it as**: A key to a room: only the one holding the key (lock) can enter the room (critical section).

### Basic Properties

| Property             | Description                              |
|----------------------|------------------------------------------|
| Type                 | Lock (not a counting variable)           |
| Values               | Locked (1) or Unlocked (0)               |
| Access               | Only the owner (locking process) can unlock it |
| Scope                | Works in mutual exclusion, for 1 resource/user |
| Implementation Style | OS-level primitive (kernel-based or user-level) |

### Operations

| Operation | Description                              |
|-----------|------------------------------------------|
| lock()    | If unlocked, it locks and enters the critical section. If already locked, the process waits (blocks). |
| unlock()  | Releases the lock so another process can enter. Only the process that acquired the lock can release it. |

### Mutex Working Example
**Scenario**:
```c
mutex.lock();
critical section
mutex.unlock();
```

**Execution Flow**:
1. P1 calls lock() → succeeds (mutex = locked), enters critical section.
2. P2 calls lock() → waits (mutex is already locked).
3. P1 finishes → calls unlock() → mutex becomes available.
4. P2 now proceeds → acquires lock → enters critical section.

- **Result**: Mutual exclusion is maintained!

### Mutex vs Binary Semaphore

| Feature           | Mutex                            | Binary Semaphore                 |
|-------------------|----------------------------------|----------------------------------|
| Ownership         | Has owner                        | No owner (any process can Up)    |
| Unlocking         | Only owner can unlock            | Any process can perform V()      |
| Use-case          | Thread synchronization           | Inter-process synchronization    |
| Blocking behavior | Auto-blocks if locked            | Needs condition checking         |
| Usage simplicity  | Simple (only lock/unlock)        | Slightly more generic            |

### When to Use Mutex?
Use Mutex when:
- You need strict ownership of the lock.
- Only one thread/process should access a resource at a time.
- Synchronization is required within the same process (multi-threaded).

### Summary
- Mutex ensures mutual exclusion using lock() and unlock().
- Only the thread that locked the mutex can unlock it.
- It is simpler and safer than binary semaphore for thread-level locking.
- Ideal for use in multi-threaded programs (like C++ std::mutex or POSIX threads).