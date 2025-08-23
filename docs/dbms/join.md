# Joins in DBMS

**Author:** Abhinav P Pradeep 

## Natural Join

### Definition
A Natural Join:
- Automatically joins tables using all common attributes (i.e., same name and compatible data type).
- Performs an equi join implicitly on those common columns.
- Removes duplicate columns in the result.

**Join Concept**:
- Join = Cartesian Product (m × n rows) + Matching rows based on some condition.
- In Natural Join, the condition is: `WHERE Table1.common_column = Table2.common_column` (done implicitly).

**Syntax**:
```sql
SELECT column_list
FROM Table1
NATURAL JOIN Table2;
```

**Condition**:
- The common column(s) must have the same name in both tables.

### Example
**Tables**:

**Employee Table**:

| EmpNo | EmpName | DeptNo |
|-------|---------|--------|
| 1     | Ram     | 10     |
| 2     | Varun   | 20     |
| 3     | Ravi    | NULL   |
| 4     | Amrit   | 30     |

**Department Table**:

| DeptNo | DeptName  |
|--------|-----------|
| 10     | HR        |
| 20     | IT        |
| 30     | Marketing |

**Natural Join Query**:
```sql
SELECT EmpName, DeptName
FROM Employee
NATURAL JOIN Department;
```

**Behind the Scenes**:
Equivalent to:
```sql
SELECT EmpName, DeptName
FROM Employee E, Department D
WHERE E.DeptNo = D.DeptNo;
```

**Result**:

| EmpName | DeptName  |
|---------|-----------|
| Ram     | HR        |
| Varun   | IT        |
| Amrit   | Marketing |

**Note**: Ravi is excluded because his DeptNo is NULL (no matching department).

### Key Rules of Natural Join

| Rule                          | Explanation                                      |
|-------------------------------|--------------------------------------------------|
| Common columns must exist      | Columns should have same name & data type        |
| Works like Equi Join          | But you don’t specify condition manually         |
| Removes duplicate columns     | Only one copy of common column appears in output |
| Ignores rows without match    | Works like inner join — no match → no output row |

### Real-World Scenarios
- Employee & Department: Joining employees to their departments.
- Student & Course: Linking student enrollment with course details.
- Customer & Orders: Getting customer names with their orders.

### If Column Names Differ
If Employee.DeptNo is named Dept_ID in one table and DeptNo in another, you cannot use Natural Join directly. Instead:
- Rename column with alias or ALTER TABLE.
- Or use Equi Join with an explicit condition.

### Exam Tip
Natural Join:
- Automatic equi join.
- On common column(s).
- With column duplication removed.
- No control over join condition manually.

### Equivalent SQL Forms

| Natural Join Syntax         | Equivalent to This                              |
|----------------------------|------------------------------------------------|
| T1 NATURAL JOIN T2         | T1 JOIN T2 ON T1.col = T2.col (for all common cols) |
| T1 JOIN T2 USING (col)     | More explicit natural join on one column        |

### Related Join Types

| Type         | Matching          | Unmatched Rows       | Duplicate Columns |
|--------------|-------------------|----------------------|-------------------|
| Natural Join | Auto              | No                   | Removed           |
| Inner Join   | Manual            | No                   | Can stay          |
| Left Join    | Manual            | Yes (left)           | Can stay          |
| Right Join   | Manual            | Yes (right)          | Can stay          |
| Full Outer   | Manual            | Yes (both)           | Can stay          |

## Equi Join

### Definition
An Equi Join:
- Combines rows from two or more tables based on a condition involving equality (=).
- The condition is usually on a common attribute (but can be on any comparable attribute).
- Unlike Natural Join, duplicate columns are not removed.
- Join = Cartesian Product + Selection Condition.

### Example Tables

**Employee Table**:

| EmpNo | EmpName | Address    | DeptNo |
|-------|---------|------------|--------|
| 1     | Ram     | Delhi      | 1      |
| 2     | Varun   | Chandigarh | 2      |
| 3     | Ravi    | Mumbai     | NULL   |
| 4     | Amrit   | Delhi      | 3      |

**Department Table**:

| DeptNo | Location |
|--------|----------|
| 1      | Delhi    |
| 2      | Pune     |
| 3      | Patna    |

**Question**: Find the employee names who work in a department located in the same city as their address.

### Approach with Equi Join
**Steps**:
1. Use both tables: Employee (for names) and Department (for location).
2. Perform a Cartesian product (`FROM Employee, Department`).
3. Add two conditions:
   - `Employee.DeptNo = Department.DeptNo` (ensures valid department mapping).
   - `Employee.Address = Department.Location` (checks address = location).

**SQL Query**:
```sql
SELECT E.EmpName
FROM Employee E, Department D
WHERE E.DeptNo = D.DeptNo
  AND E.Address = D.Location;
```

**Output**:

| EmpName |
|---------|
| Ram     |

**Explanation**:
- Ram (EmpNo 1) lives in Delhi, works in Dept 1 (Delhi) → Match.
- Varun lives in Chandigarh, works in Dept 2 (Pune) → No match.
- Ravi has no department → No match.
- Amrit lives in Delhi, works in Dept 3 (Patna) → No match.

### Comparison: Equi Join vs Natural Join

| Feature             | Equi Join                              | Natural Join                           |
|--------------------|----------------------------------------|----------------------------------------|
| Condition syntax   | Manual: A.col = B.col                  | Automatic on common columns            |
| Column duplication | Retained                               | Removed                               |
| Flexibility        | High (join on any attribute)           | Limited to matching column names       |
| Query complexity   | Explicit                               | Simpler for identical columns          |

**Exam Tip**:
- Equi Join allows joining on any attribute using =. When joining on common keys, it becomes logically similar to Natural Join but with more control and retained duplicates.

## Outer Joins

### Left Outer Join (LOJ)

**Definition**:
- Returns all matching rows (like Natural Join) + unmatched rows from the left table.
- Adds NULL in columns from the right table where there’s no match.

**Example**:

**Employee Table (Left)**:

| EmpNo | EmpName | DeptNo |
|-------|---------|--------|
| E1    | Varun   | D1     |
| E2    | Amrit   | D2     |
| E3    | Ravi    | D1     |
| E4    | Nitin   | NULL   |

**Department Table (Right)**:

| DeptNo | DeptName | Location |
|--------|----------|----------|
| D1     | IT       | Delhi    |
| D2     | HR       | Hyd      |
| D3     | Finance  | Pune     |
| D4     | Testing  | Noida    |

**Query**:
```sql
SELECT E.EmpNo, E.EmpName, D.DeptName, D.Location
FROM Employee E
LEFT OUTER JOIN Department D
ON E.DeptNo = D.DeptNo;
```

**Output**:

| EmpNo | EmpName | DeptName | Location |
|-------|---------|----------|----------|
| E1    | Varun   | IT       | Delhi    |
| E2    | Amrit   | HR       | Hyd      |
| E3    | Ravi    | IT       | Delhi    |
| E4    | Nitin   | NULL     | NULL     |

### Right Outer Join (ROJ)

**Definition**:
- Returns all matching rows (like Natural Join) + unmatched rows from the right table.
- Adds NULL in columns from the left table where there’s no match.

**Query**:
```sql
SELECT E.EmpNo, E.EmpName, D.DeptName, D.Location
FROM Employee E
RIGHT OUTER JOIN Department D
ON E.DeptNo = D.DeptNo;
```

**Output**:

| EmpNo | EmpName | DeptName | Location |
|-------|---------|----------|----------|
| E1    | Varun   | IT       | Delhi    |
| E2    | Amrit   | HR       | Hyd      |
| E3    | Ravi    | IT       | Delhi    |
| NULL  | NULL    | Finance  | Pune     |
| NULL  | NULL    | Testing  | Noida    |

### Full Outer Join (FOJ)

**Definition**:
- Returns all rows from both tables (matched and unmatched).
- Combines Left Outer Join + Right Outer Join.
- NULL where no match is found in the opposite table.

**Query** (Note: Not supported in MySQL directly):
```sql
SELECT E.EmpNo, E.EmpName, D.DeptName, D.Location
FROM Employee E
FULL OUTER JOIN Department D
ON E.DeptNo = D.DeptNo;
```

**Output**:

| EmpNo | EmpName | DeptName | Location |
|-------|---------|----------|----------|
| E1    | Varun   | IT       | Delhi    |
| E2    | Amrit   | HR       | Hyd      |
| E3    | Ravi    | IT       | Delhi    |
| E4    | Nitin   | NULL     | NULL     |
| NULL  | NULL    | Finance  | Pune     |
| NULL  | NULL    | Testing  | Noida    |

### Venn Diagram Intuition

| Join Type        | Covered Set          |
|------------------|----------------------|
| Natural Join     | A ∩ B                |
| Left Outer Join  | A ∩ B + (A - B)     |
| Right Outer Join | A ∩ B + (B - A)     |
| Full Outer Join  | A ∪ B                |

**Tips**:
- Left/Right Outer Join = Natural Join + unmatched from respective side.
- Always consider which table is “LEFT” or “RIGHT” in the SQL query.
- NULLs in output indicate no match in that side’s table.