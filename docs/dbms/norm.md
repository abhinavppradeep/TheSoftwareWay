# Normalization in DBMS

**Author:** Abhinav P Pradeep 

## What is Normalization?

Normalization is a technique in Database Management Systems (DBMS) used to remove or reduce redundancy (duplicate data) from a table.

- **Redundancy**: Repeated data.
- **Goal**: Better storage, faster updates, no data inconsistency.

## Types of Redundancy

### Row-Level Redundancy
- **Description**: Same row repeated more than once.
- **Example Table**:

| SID | Name | Age |
|-----|------|-----|
| 1   | Raj  | 20  |
| 2   | Ravi | 21  |
| 1   | Raj  | 20  |

- **Fix**: Use a Primary Key (SID) to ensure:
  - Uniqueness.
  - Not Null.
- **Result**: Duplicate rows are not allowed.

### Column-Level Redundancy
- **Description**: Same data repeated across columns in multiple rows.
- **Example Table**:

| SID | S.Name | CID | C.Name | FID | F.Name | Salary |
|-----|--------|-----|--------|-----|--------|--------|
| 1   | Raj    | C1  | DBMS   | F1  | Rahul  | 30000  |
| 2   | Ravi   | C1  | DBMS   | F1  | Rahul  | 30000  |
| 3   | Aman   | C2  | OS     | F2  | Neha   | 35000  |

- **Problem**: Course name, Faculty name, and Salary are repeated.

## Anomalies Due to Redundancy

Anomalies are problems that arise due to bad table design.

### Insertion Anomaly
- **Issue**: Cannot insert a new course or faculty without student info.
- **Example**: Want to insert:
  - CID: C10
  - C.Name: MBBS
- **Problem**: Cannot leave SID empty since it’s a Primary Key, forcing fake/dummy data.

### Deletion Anomaly
- **Issue**: Deleting a student may remove faculty/course info unintentionally.
- **Example**: Delete SID=2.
- **Result**: Also lose:
  - Course C1.
  - Faculty F1.
- **Problem**: Loss of critical info just by deleting one student.

### Update Anomaly
- **Issue**: Updates need to happen in multiple rows manually.
- **Example**: Update F1’s salary from 30000 to 40000.
- **Problem**: F1 appears in multiple rows, so must update all, which is prone to error and inconsistency.

## How Normalization Helps

Normalization breaks the big table into smaller, related tables using keys to avoid anomalies.

**Split into Three Tables**:

**Student Table**:

| SID | S.Name |
|-----|--------|
| 1   | Raj    |
| 2   | Ravi   |

**Course Table**:

| CID | C.Name |
|-----|--------|
| C1  | DBMS   |
| C2  | OS     |

**Faculty Table**:

| FID | F.Name | Salary |
|-----|--------|--------|
| F1  | Rahul  | 40000  |
| F2  | Neha   | 35000  |

**Benefits**:
- Insert new course/faculty directly.
- Delete student without losing faculty/course info.
- Update faculty salary in only one place.

**Primary Keys in Normalized Tables**:

| Table   | Primary Key |
|---------|-------------|
| Student | SID         |
| Course  | CID         |
| Faculty | FID         |

**Summary Table**:

| Problem            | Cause                            | Normalization Fix                |
|--------------------|----------------------------------|----------------------------------|
| Insertion Anomaly  | Cannot insert partial info       | Split into independent tables    |
| Deletion Anomaly   | Unintended loss of data          | Maintain separate entity tables  |
| Update Anomaly     | Repetition causes inconsistency  | Update in only one location      |

**Key Takeaways**:
- Normalize to avoid anomalies.
- Use Primary Keys to uniquely identify rows.
- Avoid storing repeated data in the same table.
- Aim for a clean, modular table structure.

## Closure Method

### What is the Closure Method?

The Closure Method is used to find all Candidate Keys in a relation based on given Functional Dependencies (FDs).

**Why Important?**:
- Essential for 2NF, 3NF, BCNF.
- Used in GATE, interviews, and real database design.

### Key Definitions

| Term             | Meaning                                                  |
|------------------|----------------------------------------------------------|
| Closure (X⁺)     | All attributes that can be functionally determined by X   |
| Candidate Key    | A minimal set of attributes whose closure includes all table attributes |
| Super Key        | A set that includes a candidate key (can have extra attributes) |
| Prime Attribute  | Used in any candidate key                                |
| Non-Prime Attribute | Not used in any candidate key                         |

### Step-by-Step: How to Use Closure Method

**Goal**: Find all candidate keys.

**Step 1: Identify All Functional Dependencies**:
- Example:
  - Relation: R(A, B, C, D)
  - FDs:
    - A → B
    - B → C
    - C → D

**Step 2: Take Closure of Attributes**:
- **A⁺ (Closure of A)**:
  - A⁺ = A (reflexive).
  - A → B → C → D (transitive).
  - So, A⁺ = {A, B, C, D}.
  - A⁺ contains all attributes → A is a Candidate Key.
- **B⁺** = {B, C, D} → Missing A, not a Candidate Key.
- Similarly for C⁺ = {C, D}, D⁺ = {D}.

**Conclusion**:
- Candidate Key = {A}.
- Prime Attribute = A.
- Non-Prime = {B, C, D}.

**Trick to Identify Key Candidates Quickly**:
- Any attribute NOT on the RHS of any FD must be in every Candidate Key.
- Example:
  - FDs:
    - A → B
    - B → D
    - E → C
    - C → A
  - RHS: B, D, C, A → E is not on RHS.
  - E must be part of the candidate key.

### More Examples

**Example 1**:
- Relation: R(A, B, C, D)
- FDs:
  - AB → C
  - B → C
  - C → D
  - D → A
- **Closure**:
  - A⁺ = {A, B, C, D} → Candidate Key.
  - B⁺ = {B, C, D, A} → Candidate Key.
  - C⁺ = {C, D, A, B} → Candidate Key.
  - D⁺ = {D, A, B, C} → Candidate Key.
- **Result**:
  - Candidate Keys: A, B, C, D.
  - Prime Attributes: A, B, C, D.
  - Non-Prime: ∅ (phi).

**Example 2**:
- Relation: R(A, B, C, D, E)
- FDs:
  - A → B
  - BC → D
  - D → A
  - E → C
- **Tip**: RHS = {B, D, A, C} → E not present, so E must be in every candidate key.
- **Closure**:
  - E⁺ = {E, C} → Not enough.
  - AE⁺:
    - A → B
    - E → C
    - BC → D
    - D → A
    - AE⁺ = {A, B, C, D, E} → Candidate Key.
  - Try DE, BE, CE:
    - DE⁺ = {D, E, C, A, B} → Candidate Key.
    - BE⁺ = {B, E, C, D, A} → Candidate Key.
- **Result**:
  - Candidate Keys: AE, DE, BE.
  - Prime Attributes: A, B, D, E.
  - Non-Prime: C.

## Functional Dependency (FD)

### What is a Functional Dependency?

Functional Dependency describes the relationship between attributes in a relation (table). It shows how one attribute (or group of attributes) can determine another.

**Format**:
- X → Y
  - X: Determinant attribute(s).
  - Y: Dependent attribute(s).
  - Meaning: "X determines Y" or "Y is functionally dependent on X".

**Example**:
- Relation: Student(SID, S.Name)
- FD: SID → S.Name
- **Meaning**:
  - One SID determines one S.Name.
  - Same name may occur with different SIDs.
  - Use SID to remove ambiguity in names like "Ranjit".

**Valid & Invalid FD Cases**:

| Case # | S.Name (Y)       | SID (X) | Valid/Invalid | Explanation                              |
|--------|------------------|---------|---------------|------------------------------------------|
| 1      | Ranjit, Ranjit   | 1, 1    | ✅ Valid       | Same SID, repeated entry                 |
| 2      | Ranjit, Ranjit   | 1, 2    | ✅ Valid       | Two different students with same name     |
| 3      | Ranjit, Varun    | 1, 2    | ✅ Valid       | Two different students                   |
| 4      | Ranjit, Varun    | 1, 1    | ❌ Invalid     | One SID cannot determine two different names |

- If the determinant (LHS) is the same but the dependent (RHS) differs → Invalid FD.

### Types of Functional Dependencies

#### Trivial Functional Dependency
- X → Y is trivial if Y ⊆ X.
- Always valid.
- **Examples**:
  - SID → SID
  - {SID, S.Name} → SID
- **Intersection Rule**:
  - If X ∩ Y ≠ ∅ → Trivial FD.

#### Non-Trivial Functional Dependency
- X → Y is non-trivial if Y ⊈ X.
- Requires validation.
- **Examples**:
  - SID → S.Name
  - EmpID → EmpName, City
- **Intersection Rule**:
  - If X ∩ Y = ∅ → Non-Trivial FD.

### Properties of Functional Dependency

| Property         | Rule                           | Notes                                      |
|------------------|--------------------------------|--------------------------------------------|
| Reflexivity      | If Y ⊆ X, then X → Y           | Always true                                |
| Augmentation     | If X → Y, then XZ → YZ         | Add same attribute to both sides           |
| Transitivity     | If X → Y and Y → Z, then X → Z | Very important                             |
| Union            | If X → Y and X → Z, then X → YZ | Combine RHS                               |
| Decomposition    | If X → YZ, then X → Y and X → Z | Decompose RHS only                        |
| Pseudo-Transitivity | If X → Y and WY → Z, then WX → Z | Mix rules                              |
| Composition      | If X → Y and Z → W, then XZ → YW | Combine both sides                        |

**Important Rule**:
- Never decompose LHS. For example:
  - ❌ XY → Z ⇒ X → Z or Y → Z (Not always valid!).

**Summary**:

| Concept                | Quick Tip                              |
|------------------------|----------------------------------------|
| Functional Dependency  | X → Y means X determines Y             |
| Valid FD               | One value of X → One value of Y        |
| Invalid FD             | One value of X → Multiple values of Y  |
| Trivial FD             | Y ⊆ X                                  |
| Non-Trivial FD         | Y ⊈ X                                  |
| Validate FDs           | Only for non-trivial ones              |
| Reflexivity & Transitivity | Most used in exams & practice       |

## Normal Forms

### First Normal Form (1NF)

**Definition**:
A relation (table) is in 1NF if:
1. All attributes contain only atomic (indivisible) values.
2. There are no repeating groups or multivalued attributes.

**In Simple Words**:
- Every cell in the table should hold only one value, not lists, sets, or groups.

**Example: Not in 1NF**:

| StudentID | Name | Courses           |
|-----------|------|-------------------|
| 101       | Alice | DBMS, OS, Networks |
| 102       | Bob  | DBMS, DSA         |

**Violation**:
- Column Courses has multiple values in a single cell (not atomic).

**Converted to 1NF**:

| StudentID | Name  | Course   |
|-----------|-------|----------|
| 101       | Alice | DBMS     |
| 101       | Alice | OS       |
| 101       | Alice | Networks |
| 102       | Bob   | DBMS     |
| 102       | Bob   | DSA      |

**Now**:
- Each cell contains a single value.
- No multivalued columns.
- Table is in 1NF.

**Common Mistakes That Violate 1NF**:

| Violation             | Example                      |
|-----------------------|------------------------------|
| Multivalued attributes | Courses = {DBMS, OS}         |
| Composite values in one cell | Name = John, Smith     |
| Repeating columns      | Phone1, Phone2, Phone3       |

**Quick Checklist for 1NF**:

| Check                                    | Passes 1NF? |
|------------------------------------------|-------------|
| Does every attribute contain atomic values? | ✅ Yes       |
| Any attribute storing multiple values?    | ❌ No        |
| Any repeating columns (e.g., Phone1, Phone2)? | ❌ No      |

**Mnemonic**:
- "1 Cell = 1 Value" → That’s 1NF.

### Second Normal Form (2NF)

**Definition**:
A table is in Second Normal Form (2NF) if:
1. The table is in First Normal Form (1NF).
2. No Partial Dependency exists.

**Key Definitions**:
- **Partial Dependency**: A non-prime attribute is partially dependent on a candidate key if it’s dependent on a subset (proper part) of a composite candidate key.
  - Only applies when the candidate key is composite (has 2 or more attributes).
  - A non-prime attribute depends only on part of that key.
- **Prime Attribute**: An attribute that is part of a candidate key.
- **Non-Prime Attribute**: An attribute that is not part of any candidate key.

**2NF Rule (In Simple Words)**:
- All non-prime attributes should be fully functionally dependent on the entire candidate key, not just a part of it.

**Step-by-Step Check for 2NF**:
1. Check for 1NF:
   - No multivalued or repeating attributes.
2. Identify Candidate Key(s):
   - Use Closure Method to find candidate keys.
3. List Prime & Non-Prime Attributes:
   - Prime: Used in forming candidate key.
   - Non-Prime: Everything else.
4. Check Functional Dependencies:
   - If LHS is a proper subset of a candidate key and RHS is a non-prime attribute → Partial Dependency.
   - If even one partial dependency exists → Table not in 2NF.

**Example: Customer Table**:

| CustomerID | StoreID | Location  |
|------------|---------|-----------|
| 1          | 1       | Delhi     |
| 2          | 1       | Delhi     |
| 3          | 2       | Bangalore |
| 4          | 3       | Mumbai    |

**FDs**:
- Candidate Key: {CustomerID, StoreID}
- FD: StoreID → Location

**Check**:
- Location is a non-prime attribute.
- StoreID is a part of the candidate key.
- Location is partially dependent.
- **Result**: Not in 2NF.

**To Convert to 2NF**:
- Break into two tables:
  1. **Customer Table**:

| CustomerID | StoreID |
|------------|---------|
| 1          | 1       |
| 2          | 1       |
| 3          | 2       |
| 4          | 3       |

  2. **Store Table**:

| StoreID | Location  |
|---------|-----------|
| 1       | Delhi     |
| 2       | Bangalore |
| 3       | Mumbai    |

**Now**:
- Location is dependent on the full key (StoreID).
- No partial dependencies remain.

**GATE Question Example**:
- Relation: R(A, B, C, D, E, F)
- FDs:
  1. C → F
  2. E → A
  3. EC → D
  4. A → B
- **Step 1: Candidate Key**:
  - RHS attributes = {F, A, D, B}.
  - LHS = EC must be key (closure of EC → all attributes).
  - Candidate Key = EC.
- **Step 2: Prime / Non-Prime Attributes**:
  - Prime: E, C.
  - Non-Prime: A, B, D, F.
- **Step 3: Check for Partial Dependencies**:

| FD        | LHS Subset of Key? | RHS Non-Prime? | Result              |
|-----------|--------------------|----------------|---------------------|
| C → F     | ✅ Yes             | ✅ Yes          | ❌ Partial Dependency |
| E → A     | ✅ Yes             | ✅ Yes          | ❌ Partial Dependency |
| EC → D    | ❌ No (full key)   | ✅ Yes          | ✅ Full Dependency   |
| A → B     | ❌ A is non-prime  | ✅ Yes          | Not partial (violates 3NF) |

- **Result**: Table is not in 2NF due to partial dependencies.

**Tips to Remember**:

| Concept                  | Tip                                    |
|--------------------------|----------------------------------------|
| 2NF Condition            | No partial dependency                  |
| Check for partial dep.   | LHS ⊂ candidate key & RHS is non-prime |
| Happens only if key is composite | True                             |
| Fixing partial dependencies | Decompose the table                  |
| Minimum requirement       | Table must be in 1NF                   |

### Third Normal Form (3NF) — E.F. Codd’s Rules

**Definition**:
A relation is in 3NF if:
1. It is already in Second Normal Form (2NF).
2. No transitive dependency exists for non-prime attributes.

**What is a Transitive Dependency?**:
- Occurs if: A → B → C.
  - A is a candidate key.
  - B is a non-prime attribute.
  - C is a non-prime attribute.
- **Problem**: A non-prime (C) is determined by another non-prime (B) — not allowed in 3NF.

**Key Definitions**:

| Term                | Meaning                              |
|---------------------|--------------------------------------|
| Prime Attribute     | Part of any candidate key            |
| Non-Prime Attribute | Not part of any candidate key        |
| Transitive Dependency | Non-prime → non-prime (indirectly via key) |

**Most Practical Condition to Check 3NF**:
- For every functional dependency (FD): X → Y
- At least one of the following must be true:
  1. X is a super key.
  2. Y is a prime attribute.
- This is the golden rule for solving MCQs or exam problems.

**Example 1**:
- Relation: Student(RollNo, State, City)
- FDs:
  - RollNo → State
  - State → City
- **Analysis**:
  - RollNo = candidate key (prime).
  - State, City = non-prime.
  - State → City = transitive dependency (non-prime → non-prime).
- **Result**: Not in 3NF.

**Example 2**:
- Relation: R(A, B, C, D)
- FDs:
  - AB → C
  - C → D
- **Candidate Key**: AB
  - AB → C (valid: key → non-prime).
  - C → D (invalid: non-prime → non-prime).
- **Result**: Not in 3NF.

**Example 3 (Valid 3NF)**:
- Relation: R(A, B, C, D)
- FDs:
  - AB → CD
  - D → A
- **Candidate Keys**: AB, DB
- **Prime Attributes**: A, B, D
- **Non-Prime Attribute**: C
- **Check each FD**:
  - AB → CD (valid: AB is a candidate key).
  - D → A (valid: A is prime attribute).
- **Result**: This is in 3NF.

**Summary to Remember 3NF**:

| What to Check                  | Valid for 3NF? |
|--------------------------------|----------------|
| Non-prime → Non-prime (transitive) | ❌ Invalid     |
| Key → Non-prime                | ✅ Valid       |
| Non-prime → Prime              | ✅ Valid       |
| Key → Anything                 | ✅ Valid       |
| Prime → Prime                  | ✅ Valid       |

**Tip**:
- Only one rule to remember: "For every FD X → Y, X must be a superkey or Y must be a prime attribute."

### Boyce-Codd Normal Form (BCNF)

**What is BCNF?**:
- A stricter version of 3NF, sometimes called 3.5NF.
- Every BCNF-compliant table is also in 3NF, but not all 3NF tables are in BCNF.

**Condition for BCNF**:
- For every functional dependency X → Y, X must be a candidate key (or a super key).
- Unlike 3NF, BCNF does not allow:
  - X being non-key (even if Y is a prime attribute).
  - Any dependency where LHS is not a candidate key.

**Steps to Check BCNF**:
1. Table must already be in 3NF.
2. For each FD, check:
   - Is the LHS a candidate key or super key?
   - If yes → FD is valid in BCNF.
   - If no → Table is not in BCNF.

**Example**:
- **Student Table**:
  - Attributes: RollNo, Name, VoterID, Age
  - FDs:
    - RollNo → Name
    - RollNo → VoterID
    - VoterID → Age
    - VoterID → RollNo
  - **Candidate Keys**: RollNo, VoterID
  - Both can uniquely identify a student.
- **BCNF Check**:

| FD                  | LHS      | Is Candidate Key? | Valid in BCNF? |
|---------------------|----------|-------------------|----------------|
| RollNo → Name       | RollNo   | ✅ Yes            | ✅ Valid       |
| RollNo → VoterID    | RollNo   | ✅ Yes            | ✅ Valid       |
| VoterID → Age       | VoterID  | ✅ Yes            | ✅ Valid       |
| VoterID → RollNo    | VoterID  | ✅ Yes            | ✅ Valid       |

- **Conclusion**: All LHS are candidate keys → Table is in BCNF.

**BCNF vs 3NF: Core Difference**:

| Feature                  | 3NF                                      | BCNF                                    |
|--------------------------|------------------------------------------|-----------------------------------------|
| Condition                | LHS is a super key OR RHS is a prime attribute | LHS must be a super key/candidate key |
| Handles anomalies        | Yes                                      | Yes, but better than 3NF                |
| All 3NF ⇒ BCNF?          | ❌ Not always                             | ✅ All BCNF are in 3NF                  |
| Example Violation Case   | If LHS is not a super key but RHS is prime | Such case is not allowed in BCNF       |

**Normal Forms Relationship**:
- BCNF ⊂ 3NF ⊂ 2NF ⊂ 1NF
- **Visual Tip**:
  - Imagine each normal form as a circle inside another.
  - BCNF is the deepest and strictest.

**Common Interview Questions**:
- **Q1**: Is every 3NF table in BCNF? → ❌ No
- **Q2**: Is every BCNF table in 3NF? → ✅ Yes
- **Q3**: What’s the key condition for BCNF? → LHS of every FD must be a candidate key

### Example: Normalizing a Table

**0NF (Unnormalized Form)**:

| RollNo | Name | Courses         | Dept | HOD    |
|--------|------|-----------------|------|--------|
| 101    | Alice | DBMS, OS        | CSE  | Dr. Rao |
| 102    | Bob  | DBMS, Networks  | CSE  | Dr. Rao |

**Issue**: Courses is multivalued, cells are not atomic.

**1NF — Remove Multivalued Attributes**:

| RollNo | Name | Course   | Dept | HOD    |
|--------|------|----------|------|--------|
| 101    | Alice | DBMS     | CSE  | Dr. Rao |
| 101    | Alice | OS       | CSE  | Dr. Rao |
| 102    | Bob  | DBMS     | CSE  | Dr. Rao |
| 102    | Bob  | Networks | CSE  | Dr. Rao |

**Now**:
- All attributes are atomic.
- No repeating or multivalued fields.

**2NF — Remove Partial Dependencies**:
- Candidate Key: (RollNo, Course)
- Partial dependency: RollNo → Name, Dept
- Break into two tables:
  1. **Enrollment Table**:

| RollNo | Course   |
|--------|----------|
| 101    | DBMS     |
| 101    | OS       |
| 102    | DBMS     |
| 102    | Networks |

  2. **Student Table**:

| RollNo | Name | Dept |
|--------|------|------|
| 101    | Alice | CSE |
| 102    | Bob  | CSE |

**Now**:
- Every non-prime attribute fully depends on the whole key.
- No partial dependencies.

**3NF — Remove Transitive Dependencies**:
- FD: Dept → HOD (transitive via RollNo → Dept).
- Break again:
  - **Department Table**:

| Dept | HOD    |
|------|--------|
| CSE  | Dr. Rao |
| IT   | Dr. Iyer |

**Final Tables after 3NF**:
1. Enrollment(RollNo, Course)
2. Student(RollNo, Name, Dept)
3. Department(Dept, HOD)

**Now**:
- No transitive dependencies.
- All non-prime attributes directly depend on candidate keys.

**Why BCNF is Needed (Even After 3NF)**:
- 3NF allows: "If RHS is a prime attribute, the LHS can be non-key."
- This is dangerous because it allows functional control from a non-superkey.

**Classic Example Where 3NF Fails**:

| Student | Course | Instructor |
|---------|--------|------------|
| A       | DBMS   | Rao        |
| B       | DBMS   | Rao        |
| A       | OS     | Rao        |

**FDs**:
- Student, Course → Instructor (valid: candidate key).
- Instructor → Course (invalid: not OK!).

**Why It Fails in 3NF**:
- Instructor → Course:
  - LHS = not a candidate key.
  - RHS (Course) = prime → 3NF valid.
  - But Instructor decides part of the key → BAD DESIGN.
- **Result**: In 3NF, but not in BCNF.

**Fix with BCNF: Decompose**:
1. **InstructorCourse Table**:

| Instructor | Course |
|------------|--------|
| Rao        | DBMS   |
| Rao        | OS     |

2. **StudentInstructor Table**:

| Student | Instructor |
|---------|------------|
| A       | Rao        |
| B       | Rao        |

**Now**:
- All FDs obey: LHS is a superkey in their table.
- This is BCNF compliant.

**Summary Table**:

| Normal Form | Problem Fixed              | Rule                                      |
|-------------|----------------------------|-------------------------------------------|
| 1NF         | Multivalued attributes     | Atomic values only (no sets/lists in a cell) |
| 2NF         | Partial dependency         | Full functional dependency on entire key  |
| 3NF         | Transitive dependency      | Non-prime depends only on superkey directly |
| BCNF        | Non-superkey determinants  | Every determinant is a superkey           |