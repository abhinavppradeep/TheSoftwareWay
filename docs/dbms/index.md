# Database Fundamentals

**Author:** Abhinav P Pradeep 

## SQL Commands

### Data Definition Language (DDL)

DDL is used to define or change the structure/schema of database objects like tables, views, and indexes. DDL commands are auto-committed, meaning changes cannot be rolled back.

| Command   | Description                                           |
|-----------|-------------------------------------------------------|
| CREATE    | Creates new table, view, index, etc.                   |
| DROP      | Deletes table or database permanently                 |
| ALTER     | Modifies structure (e.g., add/remove column)          |
| TRUNCATE  | Deletes all rows without logging individual deletions |
| RENAME    | Renames a table or column                             |

**Note**: DDL commands are auto-committed. Changes cannot be rolled back.

### Data Manipulation Language (DML)

DML is used to manipulate or query data inside existing tables. DML operations can be rolled back until explicitly committed.

| Command | Description                  |
|---------|------------------------------|
| SELECT  | Retrieves data               |
| INSERT  | Adds new row(s)              |
| UPDATE  | Modifies existing row(s)     |
| DELETE  | Removes specific row(s)       |

### Transaction Control Language (TCL)

TCL is used to manage transactions in a database and control their execution.

| Command          | Description                                   |
|------------------|-----------------------------------------------|
| COMMIT           | Saves all changes made during the transaction |
| ROLLBACK         | Undoes all changes made during the transaction |
| SAVEPOINT        | Creates a point to which you can roll back    |
| SET TRANSACTION  | Sets properties like isolation level          |

**Note**: TCL is used with DML, not DDL.

### Summary of SQL Commands

| Feature            | DDL | DML | TCL |
|--------------------|-----|-----|-----|
| Changes Schema?    | ✅ Yes | ❌ No | ❌ No |
| Changes Data?      | ❌ No | ✅ Yes | ⚙ Controls DML |
| Auto-Commit?       | ✅ Yes | ❌ No | ❌ No |
| Rollback?          | ❌ No | ✅ Yes | ✅ Yes |

**Example Workflow**:

```sql
BEGIN;
INSERT INTO employee VALUES (101, 'Abhinav', 'HR');
SAVEPOINT sp1;
UPDATE employee SET dept = 'Tech' WHERE id = 101;
ROLLBACK TO sp1;
COMMIT;
```

## Database Architectures

### 2-Tier vs. 3-Tier Architecture

#### What is a Tier?

A "tier" refers to a layer in the architecture of a database system:
- 2-tier = 2 layers
- 3-tier = 3 layers

These architectures define how clients interact with databases.

#### Two-Tier Architecture (Client-Server)

**Structure**:

```
Client (UI + Application) <---- JDBC/ODBC ----> Database Server
```

**Layers**:

| Layer           | Description                              |
|-----------------|------------------------------------------|
| Client (Tier 1) | Runs UI and Application Logic (API)      |
| DB Server (Tier 2) | Stores and processes data             |

**How It Works**:
1. Client has an interface (e.g., Java App).
2. Interface uses JDBC/ODBC to connect to the server.
3. Sends SQL queries.
4. Server processes queries and returns results.

**Real-life Examples**:
- IRCTC reservation at railway station (ticket clerk system).
- Visiting a bank physically and doing credit/debit from a teller system.

**Advantages**:
- Simple architecture.
- Easy to maintain (few clients, secured environment).

**Disadvantages**:

| Issue       | Why?                                      |
|-------------|-------------------------------------------|
| Scalability | Can't handle many users at once           |
| Security    | Client directly interacts with DB – risky |

#### Three-Tier Architecture

**Structure**:

```
Client (UI) <--> Business Layer (Application Server) <--> Database
```

**Layers**:

| Tier                     | Description                              |
|--------------------------|------------------------------------------|
| Tier 1 – Client          | UI: Users interact via app/browser        |
| Tier 2 – Business Layer  | Processes logic, prepares queries        |
| Tier 3 – Database Server | Stores, retrieves data                   |

**How It Works**:
1. Client uses UI (interface) – fills form, selects data.
2. Request is sent to the Business Layer.
3. Business Layer:
   - Validates data.
   - Converts requests into DB queries.
   - Sends to DB server.
4. DB sends result → Business Layer → User.

**Real-life Examples**:
- IRCTC website/app ticket booking.
- Net banking / UPI apps.
- Gmail, YouTube – All use 3-tier model.

**Advantages**:

| Feature     | Benefit                              |
|-------------|--------------------------------------|
| Scalability | Millions of users can access easily   |
| Security    | Users never touch DB directly         |
| Modular     | UI, logic, and data are separate      |

**Disadvantages**:
- Maintenance is harder (more layers = more complexity).

**Comparison Table: 2-Tier vs. 3-Tier**:

| Feature        | 2-Tier                     | 3-Tier                     |
|----------------|----------------------------|----------------------------|
| Layers         | Client + DB                | Client + Business + DB     |
| Security       | ❌ Direct DB access         | ✅ No direct DB access      |
| Scalability    | ❌ Limited users            | ✅ Massive users            |
| Maintenance    | ✅ Simple                   | ❌ Complex                  |
| Used In        | Bank counters, railway office | IRCTC website, Net banking |

**Real-world Analogies**:

**Bank**:
- 2-Tier: You visit branch → Clerk uses machine → Direct DB.
- 3-Tier: You use net banking app → App processes → Queries DB.

**Railways**:
- 2-Tier: Station ticket counter.
- 3-Tier: Booking via IRCTC app.

**Quick Recap**:
- 2-Tier = Client + DB (Simple but insecure).
- 3-Tier = Client + App Server + DB (Secure, Scalable).
- Real-use of 3-Tier: All modern web apps.
- Why move to 3-Tier? Security & Scalability.

## Database Schema

### What is a Schema?

A schema is a logical structure or blueprint of the database. It defines how data is organized and how relationships between data are handled.

**Simple Definition**:
- A schema is a logical representation of the database (not physical).
- It’s how DBMS shows the data to users, usually in the form of tables or entities.

**Schema vs. Physical Storage**:

| Aspect              | Schema (Logical)         | Physical Data           |
|---------------------|-------------------------|-------------------------|
| Format              | Tables / ER Model       | Files / Binary          |
| Stored in           | Memory (representation) | Hard disk / backend     |
| Used for            | Data design, query      | Data storage, efficiency |

**Schema Example (Student Entity)**:

| Attribute     | Data Type |
|---------------|-----------|
| Roll Number   | Integer   |
| Name          | Varchar   |
| Address       | Varchar   |

This is the logical structure (schema) of the Student table.

**Schema in SQL (Implementation)**:
We use SQL DDL commands to implement a schema.

```sql
CREATE TABLE Student (
  RollNo INT,
  Name VARCHAR(50),
  Address VARCHAR(100)
);
```

| SQL Component  | Purpose                     |
|----------------|-----------------------------|
| CREATE TABLE   | Creates schema (table)      |
| ALTER TABLE    | Modifies schema             |
| DROP TABLE     | Deletes schema              |

**Real-World Analogy**:
- Think of a schema like a blueprint of a house (design of rooms, layout).
- Actual data is like furniture and people inside it.

**Schema and ER Models**:
- In ER Model:
  - Entities like Student, Course are represented.
  - Each has attributes (e.g., name, roll no).
  - This becomes your schema.
- Schema can be for:
  - A single table.
  - Or a set of related tables (e.g., University database).

### Three-Schema Architecture

**Purpose**:
- The main goal is data abstraction and data independence.
- Users should not worry about:
  - How or where the data is stored physically.
  - Format or structure of data files.
  - Changes in database internals.
- Users interact with a logical view, not the actual data files.

**The Three Levels of Abstraction**:

#### External Schema (View Level)
- Defines how users see the data.
- **Key Points**:
  - Different users → different views.
  - Each view shows only relevant data.
  - Provides security & customization.
- **Example**:
  - Student sees: Marks, Attendance, Fee status.
  - Faculty sees: Enter marks, Student list, Apply leave.
  - Dean/Admin sees: All students, faculty records, salary info.
- This is what you see on websites like Flipkart, Gmail, Paytm — the UI view after logging in.

#### Conceptual Schema (Logical Level)
- Describes what data is stored and relationships among data.
- **Key Elements**:
  - Tables (e.g., Student, Course, Faculty, Book).
  - Columns (e.g., Roll No, Name, Age).
  - Relationships (e.g., Student ⟷ Course).
  - Constraints (e.g., Roll No is Primary Key).
- Think of this as the blueprint or ER diagram of your database.
- Used by: Database Designers.
- They define the structure but do not store actual data.

#### Internal Schema (Physical Level)
- Describes how data is stored physically on storage devices.
- **Includes**:
  - File structures.
  - Indexing.
  - Compression.
  - Partitioning (Centralized or Distributed storage).
  - Data fragmentation.
- **Examples**:
  - Data is stored in files (not tables).
  - Format: Binary, Flat files, CSV, etc.
- This level is managed by the Database Administrator (DBA).

**Analogy: Building a House**:

| Role             | Schema Level | Description                              |
|------------------|--------------|------------------------------------------|
| End User         | External     | Sees the house’s rooms and layout        |
| Architect        | Conceptual   | Creates the blueprint (room size, type, layout) |
| Contractor/Builder | Internal   | Builds the house using bricks, cement, wiring |

**Visual Diagram**:

```
          +----------------------+
          |     External View    |
          |  (Multiple user views)|
          +----------▲-----------+
                     |
          +----------▼-----------+
          |   Conceptual Schema  |
          |  (Tables & Relations)|
          +----------▲-----------+
                     |
          +----------▼-----------+
          |     Internal Schema  |
          | (Files, Indexes, HDD)|
          +----------------------+
```

### Data Independence

**Definition**:
- The ability to change the schema at one level of the database system without changing the schema at the next higher level.
- **Goal**:
  - Users can access data anytime, anywhere, without worrying about:
    - How data is stored (backend).
    - What tables or structure are involved.
    - What indexes or file structures are used.

**Why Data Independence Exists?**
- Based on the 3-Schema Architecture:
  - View Level: What the user sees (limited or full access — like app interface).
  - Conceptual Level: Logical design (tables, columns, relationships, constraints like PK, FK).
  - Physical Level: Actual storage (file structures, indexes, data structures like heap, B+ tree).

**Types of Data Independence**:

#### Logical Data Independence
- **Changes at**: Conceptual Level.
- **No effect on**: View Level.
- **Meaning**:
  - User applications don’t break even if:
    - New columns are added.
    - Existing columns are removed.
    - Tables are modified.
- **Example**:
  - User 1 adds a “Mobile Number” column to Student table.
  - User 2 still sees only "Name" and "Age".
  - App continues to work smoothly.
- **Achieved Using**:
  - Views (Virtual Tables).

```sql
CREATE VIEW student_view AS
SELECT name, age FROM Student;
```

#### Physical Data Independence
- **Changes at**: Physical Level.
- **No effect on**: Conceptual Level.
- **Meaning**:
  - Back-end storage operations don't affect the logical schema.
- **Example**:
  - Moving database from Hard Disk 1 → Hard Disk 2.
  - Changing data structure from Sequential Access → Linked Access.
  - Adding indexes or optimizing files.
- **Used For**:
  - Faster data access.
  - Efficient storage changes without user noticing.

**Key Concept: Transparency**:
- Users feel like data is right with them, always available and fast.
- In reality, DBMS handles all changes behind the scenes.
- **Real-World Example**:
  - Gmail, UMS portals, shopping apps.
  - They update and optimize daily (add columns, move storage).
  - You never notice it!

## Integrity Constraints

### What Are Integrity Constraints?

**Definition**:
- Integrity constraints are rules applied on database columns (attributes) to ensure valid, accurate, and consistent data is stored.
- They restrict invalid data from being entered into the database.
- Think of them as data quality rules applied while inserting, updating, or deleting records.

**Why Do We Need Constraints?**:
- Without constraints:
  - Any type of data can be inserted (e.g., age = -10, phone = "abc").
  - Data becomes unreliable or inconsistent.
  - Relationships between tables may break (e.g., orphan rows).
- Constraints enforce:
  - Correctness.
  - Reliability.
  - Uniqueness.
  - Consistency.

### Types of Integrity Constraints

#### Domain Constraints
- Define the valid set of values for an attribute (column).
- Applied via Data Type and Check Conditions.
- **Example**:

```sql
age INT CHECK (age > 0);
phone_number CHAR(10);
```

- **Prevents**:
  - Negative marks or age.
  - Invalid phone number lengths.
  - Wrong data types.

#### Entity Integrity Constraint
- Ensures that each record is uniquely identifiable.
- Implemented using Primary Key.
- **Rule**:
  - Primary Key must be Unique and Not Null.

```sql
PRIMARY KEY (student_id)
```

- Each entity (like Student, Employee) should have at least one unique identifier.

#### Referential Integrity Constraint
- Ensures relationship consistency between two tables.
- Enforced using Foreign Key.
- **Rule**:
  - If a row in Child Table refers to a Parent Table, that value must exist in the parent.

```sql
FOREIGN KEY (course_id) REFERENCES Courses(course_id)
```

- **Prevents**:
  - Orphan records.
  - Inserting child rows without a valid parent.

#### Unique Constraint
- Ensures that a column (or set of columns) has only unique values.
- **Example**:

```sql
email VARCHAR(100) UNIQUE
```

- Candidate keys are attributes with unique constraints.
- You can have multiple UNIQUE constraints, but only one PRIMARY KEY.

#### Not Null Constraint
- Ensures that a column cannot have NULL value.
- **Example**:

```sql
name VARCHAR(50) NOT NULL
```

- Required when a field is mandatory (e.g., user’s name, email).

**Summary Table**:

| Constraint Type          | Purpose                              | Implementation         | Example                              |
|--------------------------|--------------------------------------|------------------------|--------------------------------------|
| Domain                   | Valid data type or value range       | Data Type + CHECK      | age INT CHECK(age > 0)               |
| Entity Integrity         | Uniquely identify rows               | PRIMARY KEY            | PRIMARY KEY(student_id)              |
| Referential Integrity    | Maintain parent-child table consistency | FOREIGN KEY         | FOREIGN KEY(course_id) REFERENCES ... |
| Unique                   | Enforce uniqueness on column(s)      | UNIQUE                 | email VARCHAR(100) UNIQUE            |
| Not Null                 | Ensure value must be provided         | NOT NULL               | name VARCHAR(50) NOT NULL            |

**Mnemonic for Constraints**:
- **DUFUN-R**:
  - Domain
  - Unique
  - Foreign Key (Referential)
  - Update Integrity (via PK)
  - Not Null
  - Real-world Consistency

**Common SQL Example**:

```sql
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT CHECK (age > 0),
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES Department(dept_id)
);
```

**Interview Tip**:
- “Integrity constraints ensure your database remains clean, consistent, and relationally accurate. Without them, data becomes garbage!”
- Be ready to explain each constraint with one example.

## Keys in DBMS

### What is a Key?

- A key is an attribute (or a set of attributes) used to uniquely identify tuples (rows) in a relation (table).
- This helps avoid duplication and confusion between identical-looking rows.

**Real-life Analogy**:
- Think of keys like your Aadhaar, Roll No, or Email – they make sure you’re you, even if someone has your same name and age.

**Example: Student Table**:

| Name  | City  | Age | Roll No | Aadhaar No | Email ID          |
|-------|-------|-----|---------|------------|-------------------|
| Reddy | Delhi | 21  | 01      | XX123...   | reddy01@email.com |
| Reddy | Delhi | 21  | 03      | YY456...   | reddy03@email.com |

- Both have the same name, city, and age – but Roll No, Aadhaar, Email are different!
- These unique fields help identify them uniquely.

### Candidate Key

**Definition**:
- A Candidate Key is a minimal super key – a field (or combination of fields) that can uniquely identify every row in the table.
- Each candidate key is a strong contender to become the primary key.

**Properties**:
- Must be unique across rows.
- Must be minimal (no unnecessary extra attributes).
- A table can have multiple candidate keys.

**Examples (from Student Table)**:
- Possible Candidate Keys:
  - Roll Number
  - Aadhaar Number
  - Voter ID
  - License Number
  - Email ID
  - Phone Number
- All the above are:
  - Unique
  - Non-repeating
  - Sufficient to identify each student

### Primary Key

- A Primary Key is one selected candidate key that we use to uniquely identify each row in practice.
- **Chosen from** the candidate key set.
- **Only one** Primary Key per table.
- **Cannot be NULL**.
- Choose the most practical and consistent one, like Roll No or Aadhaar.

### Alternative Keys

- The remaining candidate keys, after choosing the primary key, are called Alternative Keys.
- They are still unique and valid – just not selected as the main identifier.

**Tips for Interviews & Exams**:
- “Primary key is always a candidate key, but not every candidate key is a primary key.”
- Always highlight minimality and uniqueness in candidate key definitions.
- Use real-world fields like PAN, Aadhaar, Roll No to explain.

### Foreign Key

**Definition**:
- A Foreign Key is an attribute (or set of attributes) in a table that references the Primary Key of another table or the same table.
- Creates a link between two tables.
- Ensures referential integrity.
- Used in relational databases to maintain data consistency.

**Formal Definition**:
- A Foreign Key (FK) is a field in one table that refers to the Primary Key (PK) in another table.
- It can:
  - Be single or composite.
  - Reference a simple or composite PK.

**Example Tables**:

**Table: Student (Base / Referenced Table)**:

| Roll_No (PK) | Name  | Address |
|--------------|-------|---------|
| 1            | John  | Delhi   |
| 2            | Priya | Mumbai  |

**Table: Course (Referencing Table)**:

| Course_ID | Course_Name | Roll_No (FK) |
|-----------|-------------|--------------|
| C101      | DBMS        | 1            |
| C102      | OS          | 2            |

- Course.Roll_No is a foreign key referencing Student.Roll_No.

**Why Use Foreign Keys?**:
- To relate data across tables.
- Prevent invalid data entry (e.g., you can't assign a course to a roll number that doesn’t exist).
- Maintains Referential Integrity.

**Common Interview Question**:
- **Which key is used to maintain Referential Integrity?**
  - **Answer**: Foreign Key.

**Oracle SQL Syntax**:

**While Creating Table**:

```sql
CREATE TABLE Course (
  Course_ID VARCHAR(10),
  Course_Name VARCHAR(50),
  Roll_No INT,
  FOREIGN KEY (Roll_No) REFERENCES Student(Roll_No)
);
```

**After Table is Created (ALTER)**:

```sql
ALTER TABLE Course
ADD CONSTRAINT FK_Roll
FOREIGN KEY (Roll_No)
REFERENCES Student(Roll_No);
```

**Important Points**:

| Concept                     | Notes                                      |
|-----------------------------|--------------------------------------------|
| FK references PK of...      | Same or another table (usually another)     |
| Composite PK?               | Yes, FK can reference composite keys       |
| Name Match Required?        | ❌ No — column names can be different       |
| Multiple FKs per table?     | ✅ Yes, a table can have multiple foreign keys |
| Multiple PKs per table?     | ❌ No, only one Primary Key per table       |
| Data Types (FK & PK)        | Must be same type (e.g., both INT)         |
| Nullability                 | FK column can be null, unlike PK which must not be null |

**Keywords to Remember**:
- **Referenced Table**: Table that holds the PK (e.g., Student).
- **Referencing Table**: Table that holds the FK (e.g., Course).
- **Referential Integrity**: Ensures FK values exist in PK table.

**Mnemonic**:
- **FK → PK**: Foreign Key always *Points to Primary Key*.
- **R.F.R.**: Referencing → Foreign Key → References → Referenced Table.

### Referential Integrity Operations

**Referenced Table: Student**:

| Operation | Will It Cause Problem? | Why? |
|-----------|------------------------|------|
| Insert    | ❌ No                  | You can add any student |
| Delete    | ⚠️ Maybe               | If that student is referenced in Course, ❗problem! |
| Update    | ⚠️ Maybe               | If you update the PK, it breaks the link in child |

**Referencing Table: Course**:

| Operation | Will It Cause Problem? | Why? |
|-----------|------------------------|------|
| Insert    | ⚠️ Maybe               | If RollNo doesn’t exist in Student → ❌ |
| Delete    | ❌ No                  | You can remove the course for a student |
| Update    | ⚠️ Maybe               | Changing RollNo to one not in Student → ❌ |

**Solutions to Avoid Integrity Problems**:

- **ON DELETE CASCADE**:
  - If a student is deleted from Student table → their entry in Course will also be deleted.
  - **SQL**:

```sql
FOREIGN KEY (RollNo) REFERENCES Student(RollNo) ON DELETE CASCADE
```

- **ON DELETE SET NULL**:
  - If student is deleted → RollNo in Course is set to NULL.
  - Only works if RollNo in Course is not a Primary Key.
  - **SQL**:

```sql
FOREIGN KEY (RollNo) REFERENCES Student(RollNo) ON DELETE SET NULL
```

- **ON DELETE NO ACTION (Default)**:
  - If RollNo is being referenced → you can’t delete it from Student.

- **Similarly for UPDATE**:

| Option              | Meaning                                     |
|---------------------|---------------------------------------------|
| ON UPDATE CASCADE   | Updates RollNo everywhere automatically     |
| ON UPDATE SET NULL  | Sets foreign key to NULL                    |
| ON UPDATE NO ACTION | Prevents update if referenced               |

**Real-Life Analogy**:
- Imagine a university:
  - Student Table = Student Directory.
  - Course Table = Enrollments.
  - If you remove a student, you must remove or update all records showing them enrolled in courses.

**Summary**:

| Action | Referenced Table (Student) | Referencing Table (Course) |
|--------|----------------------------|----------------------------|
| Insert | ✅ Safe                    | ⚠️ May Violate FK Constraint |
| Delete | ⚠️ May Violate RI          | ✅ Safe                    |
| Update | ⚠️ May Violate RI          | ⚠️ May Violate FK Constraint |

**Key Terms Recap**:
- **Foreign Key**: Attribute pointing to a primary key in another table.
- **Referential Integrity**: Ensures references between tables are valid.
- **ON DELETE/UPDATE CASCADE/SET NULL**: Ways to automatically maintain data consistency.

### Super Key

**Definition & Concept**:
- A Super Key is any set of attributes that contains a Candidate Key and can still uniquely identify tuples.
- **Super Key ⊇ Candidate Key**.

**Example**:
- If Roll No is a candidate key:
  - Super Keys = { Roll No, (Roll No, Name), (Roll No, Email), (Roll No, Age, Name) ... }
- Every super key must include at least one candidate key.
- Set like (Name, Age) is NOT a super key if it doesn’t guarantee uniqueness.

**Memory Tip**:
- Candidate = Clean & Minimal.
- Super = Super-set of candidate keys.

### Numerical & GATE-Style MCQ Concepts

**Case 1: A1 is a Candidate Key**:
- All Super Keys = 2^(n-1).
- **Why?**:
  - Fix A1 (must include candidate key).
  - Remaining (n-1) attributes → 2 choices each: include/exclude.
  - Total = 2^(n-1).

**Case 2: A1 and A2 are Candidate Keys**:
- All Super Keys = 2^(n-1) + 2^(n-1) - 2^(n-2).
- **Why?**:
  - A1’s super keys: 2^(n-1).
  - A2’s super keys: 2^(n-1).
  - Common sets (having both A1 and A2): 2^(n-2).
  - Total = Add both and subtract common.

**Case 3: Two Composite Candidate Keys**:
- Let’s say: Candidate keys: (A1, A2) and (A3, A4).
- Total attributes = n.
- Then:
  - Super Keys from A1,A2 = 2^(n-2).
  - Super Keys from A3,A4 = 2^(n-2).
  - Common sets = 2^(n-4).
  - Final = 2^(n-2) + 2^(n-2) - 2^(n-4).

**Practice MCQ (GATE-style)**:
- **Q**: If relation R has 6 attributes and 2 candidate keys (A1, A2), how many super keys are possible?
- **Answer**:
  - 2^(6-1) + 2^(6-1) - 2^(6-2) = 32 + 32 - 16 = 48.

## Entity-Relationship (ER) Model

### Purpose
- Used for conceptual/logical data design before implementation.
- Like an architectural plan before constructing a building.
- Helps understand data structure before coding.

**Real-Life Analogy**:
- You draw a floor plan before building a house to avoid costly changes later.
- In DBMS: You draw an ER diagram before writing SQL to avoid wrong design.

### Core Concepts

#### Entity
- **Definition**: Anything that exists physically or logically and can be identified.
- **Examples**:
  - Student, Course, Teacher, Department.
- Represented by a **Rectangle** in ER diagrams.

#### Attribute
- **Definition**: Properties or characteristics of an entity.
- **Examples for Student**:
  - Roll No, Name, Age, Address.
- Represented by an **Ellipse (Oval)**.
- **Key attribute** (e.g., Roll No) is underlined.

#### Relationship
- **Definition**: Association between two or more entities.
- **Example**:
  - A Student studies a Course.
- Represented by a **Diamond**.

#### ER Diagram Symbols

| Concept         | Symbol         | Example             |
|-----------------|----------------|---------------------|
| Entity          | 🔲 Rectangle    | Student, Course     |
| Attribute       | ⭕ Ellipse      | Name, Roll No       |
| Relationship    | 🔷 Diamond      | Studies, Teaches    |
| Key Attribute   | ⭕ Underlined   | Roll No             |

**Why Use ER Model?**:
- Makes it easy to communicate with non-technical stakeholders.
- Ensures clarity before coding.
- Prevents rework if requirements change.

**Example ER Diagram**:

```
[Insert ER Diagram Image Here]
![](images/er_diagram.png)
```

**Upcoming in ER Model Series**:
- Types of Attributes: Simple, Composite, Derived, Multivalued.
- Types of Relationships / Cardinality: 1:1, 1:N, N:1, M:N.

### Types of Attributes

#### Single-valued vs. Multi-valued Attributes

| Type            | Description                     | Example                | ER Diagram         |
|-----------------|---------------------------------|------------------------|--------------------|
| Single-valued   | Only one value per entity instance | Registration No, Age | Normal (single) ellipse |
| Multi-valued    | Multiple values per entity | Phone Numbers, Addresses | Double ellipse |

#### Simple vs. Composite Attributes

| Type            | Description                     | Example                            | ER Diagram                     |
|-----------------|---------------------------------|------------------------------------|--------------------------------|
| Simple          | Atomic, cannot be divided       | Age, Roll No, Gender               | Single ellipse                 |
| Composite       | Can be divided into subparts    | Name → First, Middle, Last; Address → City, State, ZIP | Parent ellipse with subparts |

#### Stored vs. Derived Attributes

| Type            | Description                     | Example                     | ER Diagram         |
|-----------------|---------------------------------|-----------------------------|--------------------|
| Stored          | Directly saved in DB            | Date of Birth, Registration No | Normal ellipse     |
| Derived         | Calculated from other data      | Age (derived from DOB)      | Dotted ellipse     |

#### Key vs. Non-Key Attributes

| Type            | Description                     | Example                     | ER Diagram         |
|-----------------|---------------------------------|-----------------------------|--------------------|
| Key             | Uniquely identifies each entity | Reg No, Roll No             | Underline the attribute |
| Non-Key         | May not be unique               | Name, Father Name, Address  | Normal ellipse     |

#### Required vs. Optional Attributes (SQL-side concept, not part of ER notation)

| Type            | Description                     | Example                     | Notes                     |
|-----------------|---------------------------------|-----------------------------|---------------------------|
| Required        | Cannot be null                  | Student Name, Reg No        | Marked as NOT NULL in schema |
| Optional        | Can be null                     | Phone Number, Email         | May be skipped in input forms |

#### Complex Attributes
- Combination of Composite + Multi-valued.
- **Example**: Address with multiple values, each containing Street, City, State.
- Represented using nested composite + multivalued ellipses.

**ER Diagram Notation Recap**:

| Attribute Type | ER Symbol            |
|----------------|----------------------|
| Single         | Simple ellipse       |
| Multi-valued   | Double ellipse       |
| Derived        | Dotted ellipse       |
| Key            | Underline            |
| Composite      | Parent with branches |

### Types of Relationships (ER to Relational Model)

**Overview**:

| Relationship Type | ER Representation           | Real-World Example                     | Table Reduction Possible? |
|-------------------|-----------------------------|----------------------------------------|---------------------------|
| One to One (1:1)  | ————————1—↔—1———————       | One employee → one department          | ✅ Yes                    |
| One to Many (1:N) | ————————1—↔—∞———————       | One customer → many orders             | ✅ Yes                    |
| Many to Many (M:N)| ————————M—↔—N———————       | Many students → many courses           | ❌ No                     |

**What is a Relationship?**:
- A relationship shows association between two entities in an ER model.
- **Examples**:
  - Employee ↔ Department.
  - Customer ↔ Order.
  - Student ↔ Course.

#### One-to-One (1:1)
- **Example**:
  - An employee works in one department.
  - A department has one employee managing it.
- **Tables**:
  - Employee(EID, Name, Age) → EID is PK.
  - Department(DID, DeptName, Location) → DID is PK.
  - Works(EID, DID) → Relationship Table.
- **Keys**:
  - EID and DID are both FKs in Works.
  - Either EID or DID can be PK in Works.
- **Special Point**:
  - If no duplication in EID or DID, then:
    - You can merge the relationship into either table.
- **Reduction**:
  - Final Tables: Employee + Department OR
  - Merge: Employee(EID, Name, Age, DID) → 2 tables total.

#### One-to-Many (1:N)
- **Example**:
  - A customer can place many orders.
  - But each order is placed by only one customer.
- **Tables**:
  - Customer(ID, Name, City) → ID is PK.
  - Order(OrderNo, Item, Cost) → OrderNo is PK.
  - Gives(ID, OrderNo, Date) → Relationship Table.
- **Keys**:
  - ID and OrderNo are both FKs in Gives.
  - But only OrderNo (on "many" side) is PK.
- **Special Point**:
  - In 1:N, the PK of "many" side becomes PK in relationship.
- **Reduction**:
  - Merge Gives + Order: Yes.
  - Final Tables: Customer, Order(ID, OrderNo, Item, Cost, Date).

#### Many-to-Many (M:N)
- **Example**:
  - A student can enroll in many courses.
  - A course can have many students.
- **Tables**:
  - Student(RollNo, Name, Age) → RollNo is PK.
  - Course(CID, Name, Credits) → CID is PK.
  - Studies(RollNo, CID) → Relationship Table.
- **Keys**:
  - RollNo and CID are both FKs.
  - Together form a composite PK.
- **Special Point**:
  - Cannot merge because neither RollNo nor CID is unique alone.
- **Reduction**:
  - No reduction possible.
  - Final Tables: Student, Course, Studies.

**Golden Rules Summary**:

| Case | Relationship Table PK | Merge Tables? |
|------|----------------------|---------------|
| 1:1  | Either side (EID or DID) | ✅ Yes (pick any one side) |
| 1:N  | PK of "Many" side (OrderNo) | ✅ Yes (merge with many) |
| M:N  | Composite (RollNo + CID) | ❌ No |

**Pro Tip for Exams**:
- “Where there is MANY, take that side’s PK as the relationship table's PK.”
- “In reduction, always try merging on the MANY side.”