# Deadlock and Banker's Algorithm

**Author:** Abhinav P Pradeep 

## What is Deadlock?

A deadlock is a situation where two or more processes are waiting for each other to release resources, but none of them proceed because the event they are waiting for never happens.

### Real-Life Analogies

| Example                     | Explanation                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| Bank Account                | You: "Open my account, then I’ll deposit money." Bank: "Deposit first, then we’ll open your account." → Both are waiting on each other → deadlock |
| Cars at a Narrow Road       | Car A and Car B face each other on a one-lane road. Neither wants to reverse. → Both block each other → deadlock |

### Technical Deadlock Example

**Resources**: R1 and R2  
**Processes**: P1 and P2  
- P1 holds R1 → requests R2  
- P2 holds R2 → requests R1  
**Result**: Both are waiting for each other to release their resource → deadlock!

### Necessary Conditions for Deadlock

All 4 conditions must be true for a deadlock to occur:

| Condition           | Meaning                                                                 | Example                                      |
|---------------------|-------------------------------------------------------------------------|----------------------------------------------|
| Mutual Exclusion    | Only one process can hold a resource at a time                           | Printer shared between processes             |
| No Pre-emption      | A resource can’t be forcibly taken; must be released voluntarily         | P1 keeps R1 until done; P2 must wait         |
| Hold and Wait       | A process holds one resource and waits for another                       | P1 holds R1 and waits for R2                 |
| Circular Wait       | Circular chain of processes waiting on each other                        | P1 → R2 → P2 → R1 → P1 (forms a loop)        |

**Note**: All 4 conditions together guarantee a deadlock.

### Circular Wait Diagram Example

- P1 holds R1 → requests R3  
- P2 holds R2 → requests R1  
- P3 holds R3 → requests R2  
This forms a loop:  
P1 → R3 → P3 → R2 → P2 → R1 → P1 → Circular Wait

### Summary

| Feature             | Description                                                        |
|---------------------|--------------------------------------------------------------------|
| Deadlock            | When processes block each other permanently due to resource wait   |
| Caused by           | Improper resource allocation + lack of handling strategies         |
| 4 Required Conditions | Mutual Exclusion, No Pre-emption, Hold & Wait, Circular Wait      |
| Detection           | Resource Allocation Graph or Wait-for Graph                        |
| Prevention/Avoidance | Break at least one of the 4 conditions                            |

## Deadlock Handling Techniques in OS

There are four major strategies for handling deadlocks:

### Deadlock Ignorance (Ostrich Method)
- **Idea**: Ignore the possibility of deadlock.
- **Real-world use**: Common in Windows, Linux, etc.
- **Based on the idea**: "Deadlocks are rare, so let’s not complicate the OS for it."
- **Named after**: The Ostrich, which hides its head in sand during a storm.

**Why used?**
- Deadlock detection/removal adds overhead.
- Systems prioritize performance/speed over rare correctness.

**Recovery**:
- When system hangs → user restarts manually.

### Deadlock Prevention
- **Philosophy**: "Prevention is better than cure!"
- **Approach**: Eliminate at least one of the 4 necessary conditions to prevent deadlock.

**Four Conditions & How to Break Them**:

| Condition           | How to Break It                                                  |
|---------------------|------------------------------------------------------------------|
| Mutual Exclusion    | Make resources sharable (not always possible, e.g., printer)      |
| No Pre-emption      | Allow OS to take away resources from a process (forcefully)       |
| Hold and Wait       | Allocate all resources at once before execution starts            |
| Circular Wait       | Impose resource ordering, only allow requests in increasing order |

**Resource Numbering for Circular Wait**:
- Assign numbers: Printer=1, Scanner=2, CPU=3...
- A process can only request in increasing order (e.g., 1 → 2 → 3)
- Never allowed to request lower-numbered resource → prevents cycle!

### Deadlock Avoidance
- **Idea**: “Before allocating, check if it’s safe to proceed.”
- Always check system state is safe before allocating resources.
- **Safe State**: There’s at least one execution order where all processes finish without deadlock.

**Algorithm Used**:
- **Banker’s Algorithm** (by Dijkstra)
  - Checks safety before granting a request.
  - Works like a loan manager in a bank (grants only if safe).

### Deadlock Detection & Recovery
- **Idea**: "Let deadlocks happen, then detect and fix."
- Allows maximum resource utilization but needs post-handling.

**Detection**:
- Use Resource Allocation Graph (RAG) or Wait-for Graph.
- Periodically check if there's a cycle → signals a deadlock.

**Recovery Methods**:

| Method               | Description                                                  |
|----------------------|--------------------------------------------------------------|
| Kill Processes       | Terminate one-by-one (lowest priority first) until deadlock is resolved |
| Resource Pre-emption | Take resource back from a process and give it to others (can cause rollback) |

**Note**: Killing or pre-emption affects performance and may cause data inconsistency.

### Summary Table

| Method                | Approach                | Real-world Usage          | Overhead | Notes                                      |
|-----------------------|-------------------------|---------------------------|----------|--------------------------------------------|
| Deadlock Ignorance    | Ignore                  | Windows, Linux            | Low      | Most used, relies on rarity                |
| Deadlock Prevention   | Eliminate Cause         | Mostly theoretical        | Medium   | Needs strict system control                |
| Deadlock Avoidance    | Predict & Avoid         | Some systems              | High     | Uses Banker’s Algorithm                    |
| Detection & Recovery  | Post-solution           | Used in some systems       | High     | Practical but may affect live processes     |

## Banker's Algorithm (Deadlock Avoidance)

### Why is it called Banker's Algorithm?
- **Analogy**: Like a banker who gives loans only if it's safe to do so (i.e., won't run out of money later), OS allocates resources only if safe.
- **Proposed by**: Dijkstra

### Purpose
- Used for Deadlock Avoidance
- Ensures safe resource allocation
- Requires advance knowledge of:
  - Total available resources
  - Maximum demand of each process
  - Current allocation
- Works only with static resource requests

### Input Requirements

| Matrix         | Meaning                              |
|----------------|--------------------------------------|
| Allocation     | Currently allocated resources to each process |
| Maximum Need   | Max resources a process may request  |
| Available      | Total – Allocated resources          |
| Need           | Maximum – Allocation                 |

### Core Concepts
- **Safe State**: A system is in a safe state if there exists a safe sequence in which all processes can finish.
- **Unsafe State**: Unsafe ≠ Deadlock, but can lead to deadlock. System cannot guarantee that all processes will complete.

### How to Calculate Need Matrix?
```
Need[i][j] = Max[i][j] - Allocation[i][j]
```

### Banker's Algorithm Steps
1. Calculate Need matrix
2. Initialize Work = Available
3. Finish[i] = false for all processes
4. Find a process such that:
   - Finish[i] == false
   - Need[i] ≤ Work
5. If found:
   - Allocate resources → Work = Work + Allocation[i]
   - Mark Finish[i] = true
   - Add process to safe sequence
6. Repeat Step 4
7. If all Finish[i] == true → Safe State
   - Else → Deadlock may occur

### Example Flow
**Process Table**:

| Process | Allocation | Max Need | Need   |
|---------|------------|----------|--------|
| P1      | 0 1 0      | 7 5 3    | 7 4 3  |
| P2      | 2 0 0      | 3 2 2    | 1 2 2  |
| P3      | 3 0 2      | 9 0 2    | 6 0 0  |
| P4      | 2 1 1      | 4 2 2    | 2 1 1  |
| P5      | 0 0 2      | 5 3 3    | 5 3 1  |

**Available**:
- Total: 10 5 7
- Allocated: 7 2 5 → Available = 3 3 2

**Safe Sequence Output**:
P2 → P4 → P5 → P1 → P3

**Important Conditions to Allocate**:
A process can be selected only if:
```
Need[i][j] ≤ Available[j] for all j
```

### Why It’s Not Practical in Real OS?
- Needs prior knowledge of max demand → not feasible
- Processes have dynamic resource requirements
- Still useful in theory + competitive exams

### Summary Chart

| Feature               | Banker's Algorithm                     |
|-----------------------|----------------------------------------|
| Category              | Deadlock Avoidance                     |
| Input Needed          | Allocation, Max, Total                 |
| Output                | Safe / Unsafe state                    |
| Used in Real OS?      | Not feasible due to dynamic needs      |
| Exam Relevance        | GATE, Interviews                       |
| Implementation        | C/C++, Simulations                     |