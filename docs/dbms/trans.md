# Transactions and Schedules in DBMS

**Author:** Abhinav P Pradeep 

## Transactions

### What is a Transaction?

A transaction is a set of operations performed together to complete a logical unit of work on a database.

**Real-Life Analogy**:
- ATM Withdrawal
- Online Fund Transfer
- Both involve multiple steps but result in one complete task.

**ATM Example – Transaction Steps**:

| Step | Action                     |
|------|----------------------------|
| 1    | Insert ATM Card            |
| 2    | Card Info is read from DB  |
| 3    | Choose language, account type |
| 4    | Enter amount               |
| 5    | Enter PIN                  |
| 6    | Cash dispensed             |
| 7    | SMS confirmation sent      |
| 8    | Final Message: “Transaction Successful” |

**Note**: All these steps together form one transaction.

### Database Transaction Definition

A transaction changes the database or reads data from it.

**Operations in a DB Transaction**:
1. **Read(A)**: Access data from DB (hard disk → RAM).
2. **Write(A)**: Modify data in RAM.
3. **Commit**: Save changes permanently to hard disk.

**Online Transaction Example**:

| Step                     | DB Operation |
|--------------------------|--------------|
| Login (username + password) | Read       |
| Select Beneficiary       | Read         |
| Enter amount + OTP + transaction password | Write |
| Final Confirmation       | Commit       |

### Detailed DB Transaction Example

**Scenario**: Transfer ₹500 from A (₹1000) to B (₹2000).

**Step-by-Step Breakdown**:

| Step | DB Action       | RAM Value         | Notes                     |
|------|-----------------|-------------------|---------------------------|
| 1    | Read(A)         | A = 1000          | Fetched from DB to RAM    |
| 2    | A = A - 500     | A = 500           | Arithmetic Op in RAM      |
| 3    | Write(A)        | A updated in RAM  | DB unchanged yet          |
| 4    | Read(B)         | B = 2000          | From DB to RAM            |
| 5    | B = B + 500     | B = 2500          | Arithmetic Op in RAM      |
| 6    | Write(B)        | B updated in RAM  | DB unchanged yet          |
| 7    | Commit          | A=500, B=2500 in DB | Permanent Save          |

**Note**: CPU never directly works on disk; it uses RAM for operations.

### Summary of Key Operations

| Operation | Meaning                              |
|-----------|--------------------------------------|
| Read(X)   | Fetch data from DB to RAM            |
| Write(X)  | Change value in RAM                  |
| Commit    | Save changes to DB permanently       |
| Rollback  | Undo uncommitted changes             |

### Core Concept

A transaction consists of Read, Write, and Commit (or Rollback) operations, ensuring:
- All-or-Nothing Execution.
- Data remains consistent.
- Reliability in systems like banking, e-commerce, etc.

## ACID Properties of Transactions

**ACID**: Atomicity + Consistency + Isolation + Durability.

These properties ensure safe, correct, and reliable transaction handling in DBMS.

**Why ACID Properties?**:
- Backend developers enforce them to ensure correctness and reliability in systems like Paytm, BHIM, or ATMs.

### 1. Atomicity – “All or None”

**Meaning**:
- A transaction must either complete fully or fail completely (no partial changes allowed).

**Real-Life Analogy (ATM)**:
- You inserted card, selected amount, entered PIN, but internet drops after OTP.
- DBMS rolls back the entire transaction.
- No “90% done” — it starts from scratch next time.

**Key Concept**:
- If a transaction has 100 operations and fails at 99, ALL 99 are undone.
- A failed transaction never resumes; it always restarts.

### 2. Consistency – “Data Valid Before and After”

**Meaning**:
- The total data (e.g., total money) must remain valid before and after the transaction.

**Example**:
- Before: A = ₹2000, B = ₹3000 → Total = ₹5000.
- Transaction: Transfer ₹1000 from A to B.
- After: A = ₹1000, B = ₹4000 → Total = ₹5000 (Consistent).

**Inconsistent Case**:
- Internet lost after ATM processing.
- ₹5000 debited from account, but no cash received.
- DB becomes inconsistent.
- Consistency ensures database rules are never violated.

### 3. Isolation – “No Interference Between Transactions”

**Meaning**:
- Each transaction should run as if it’s the only one in the system.

**Real-World Scenario**:
- T1 reads A.
- Meanwhile, T2 writes to A.
- T1 now sees inconsistent/partial data.

**Conceptual Goal**:
- Convert a parallel schedule (T1 → T2 → T1 → T2 → T1) into a serial one (T1 → T2 or T2 → T1).
- If possible → Isolation is preserved.
- If not → Data inconsistency may occur.

**Note**: Explored further in Conflict/View Serializability.

### 4. Durability – “Once Committed, Forever Saved”

**Meaning**:
- Once a transaction commits, the changes must be permanent and survive system failures, power off, etc.

**Analogy**:
- You save a movie in D: drive.
- Next day, laptop boots up → movie is still there.

**In DBMS**:
- If A = ₹1000 and B = ₹4000 after a transaction, restarting the server tomorrow must still show A = 1000, B = 4000.
- Durability is achieved by saving data to non-volatile storage (like hard disk).

### Summary Table

| Property     | Key Idea                          | Real-Life Analogy                              |
|--------------|-----------------------------------|------------------------------------------------|
| Atomicity    | All or none                       | ATM transaction fails → starts from scratch     |
| Consistency  | DB remains valid                  | Money in account + cash withdrawn = original total |
| Isolation    | No interference                   | Each transaction acts like it’s alone           |
| Durability   | Permanent changes                 | Saved file stays on disk even after restart     |

### Mini Quiz

1. What happens if a transaction fails after 99 operations?
2. If A=2000, B=3000 before, and after transaction A=1000, B=4000 — is consistency preserved?
3. Can we rearrange a parallel transaction schedule into a serial one? Why?
4. What ensures that transaction changes stay even after a crash?

**Answers**:
1. Entire transaction rolls back — nothing is saved.
2. Yes, because total remains 5000 → Consistency preserved.
3. Yes, if it’s serializable, isolation is achieved.
4. Durability — by saving to hard disk.

## Transaction States in DBMS

When a transaction is executed in a DBMS, it goes through the following states:

### 1. Active State
- The transaction has started and is performing read/write operations.
- Example: Reading data from the database and doing calculations like A = A - 50.
- Data is in RAM (volatile memory).
- **Transition**: After performing all operations except commit → moves to Partially Committed.

### 2. Partially Committed State
- All operations except the final commit are successfully executed.
- Changes are still in RAM, not in the permanent database.
- One last step (COMMIT) remains to make changes permanent.

### 3. Committed State
- The commit operation has been executed.
- All changes made by the transaction are now saved permanently in the hard disk (non-volatile storage).
- The transaction is safe, even if the system crashes after this.

### 4. Failed State
- The transaction has encountered an error before commit.
- Causes: Power failure, network issues, system crash, user mistake (e.g., refreshing payment page).
- Example: You entered OTP, but clicked refresh → transaction failed.

### 5. Aborted State
- A failed transaction is aborted.
- Rollback happens: All changes are undone to maintain consistency.
- The transaction cannot resume; it must restart from the beginning.
- After abort: Transaction can either be terminated or restarted later.

### 6. Terminated State
- The transaction has successfully finished or been aborted.
- All resources (CPU, RAM, network, locks, etc.) are deallocated.
- It is now removed from the system.

### Important Notes
- After commit, a transaction cannot fail.
- Rollback is only possible before commit.
- No partial execution is allowed (atomicity).
- In real systems like ATM, IRCTC, or UPI apps, these states ensure data correctness and consistency even in failure scenarios.

## DBMS Schedules: Serial vs Parallel

### What is a Schedule?

A schedule is a chronological sequence that defines how multiple transactions are executed in a DBMS.
- Example: T1, T2, T3 are transactions. A schedule tells the order in which their operations occur.

### Types of Schedules

#### 1. Serial Schedule
- One transaction at a time — no interleaving.
- **Execution Flow**:
  - T1 executes completely → then T2 → then T3.
  - No switching between transactions during execution.
- **Advantages**:
  - Consistency guaranteed (no interference).
  - Easy to understand and debug.
- **Disadvantages**:
  - High waiting time for other transactions.
  - Low performance and low throughput.
- **Real-Life Analogy**:
  - ATM machine with a queue: One person uses it at a time, others wait.

#### 2. Parallel Schedule (Interleaved Schedule)
- Multiple transactions execute concurrently (interleaved steps).
- **Execution Flow**:
  - T1, T2, T3 may start at the same time.
  - CPU switches between transactions (context switching).
- **Advantages**:
  - High performance.
  - Better CPU utilization.
  - High throughput: More transactions per unit time.
- **Disadvantages**:
  - May lead to inconsistency if not managed properly.
  - Requires concurrency control (e.g., conflict serializability, recoverability).
- **Real-Life Analogy**:
  - Online banking/IRCTC website: Many users transacting at the same time.
  - System handles thousands of users concurrently.

### Throughput & Performance
- **Throughput**: Number of transactions completed per unit time.
- More parallelism → More throughput → Better performance.

### Summary Table

| Feature            | Serial Schedule            | Parallel Schedule          |
|--------------------|----------------------------|----------------------------|
| Execution          | One at a time              | Interleaved                |
| Waiting time       | High                       | Low                        |
| Performance        | Low                        | High                       |
| Consistency        | High (guaranteed)          | May be inconsistent        |
| Real-world analogy | ATM queue                  | IRCTC / Online banking     |

### Key Takeaway
- Today’s DBMS systems rely on parallel schedules due to user demand for high performance and speed.
- To ensure correctness, concurrency control mechanisms (e.g., serializability, locking) are required.