# 📘 SQL Tutorial

## 📌 Overview
This tutorial introduces the core components of SQL (Structured Query Language), categorized into five key areas:
- **DDL** – Data Definition Language
- **DML** – Data Manipulation Language
- **DQL** – Data Query Language
- **TCL** – Transaction Control Language
- **DCL** – Data Control Language

---

## 📘 Data Definition Language (DDL)

**Data Definition Language (DDL)** is a subset of SQL used to define and manage the structure of database objects such as tables, columns, and data types. DDL commands shape the schema of a database and are essential for setting up and maintaining its architecture.

---

### 🔧 DDL on Tables

#### ✅ Create Tables
```sql
CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Gender CHAR(1),
    DateOfBirth DATE,
    Email VARCHAR(100),
    DepartmentID INT
);

CREATE TABLE Department (
    DepartmentID INT PRIMARY KEY,
    DepartmentName VARCHAR(100),
    HeadOfDepartment VARCHAR(100)
);

CREATE TABLE Address (
    AddressID INT PRIMARY KEY,
    StudentID INT,
    Street VARCHAR(100),
    City VARCHAR(50),
    State VARCHAR(50),
    ZipCode VARCHAR(10),
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID)
);
```

#### 🗑 Drop Tables
```sql
DROP TABLE Address;
```

#### 🧹 Truncate Table (removes all data, keeps structure)
```sql
TRUNCATE TABLE Department;
```

---

### 🧱 DDL on Columns

#### ➕ Add Column
```sql
ALTER TABLE Student
ADD PhoneNumber VARCHAR(15);
```

#### ❌ Drop Column
```sql
ALTER TABLE Student
DROP COLUMN Gender;
```

#### ✏️ Rename Column
```sql
ALTER TABLE Student
RENAME COLUMN FirstName TO GivenName;
```

---

### 🧬 DDL on Column Data Types

#### 🔄 Modify Column Data Type
```sql
ALTER TABLE Student
MODIFY Email VARCHAR(150);  -- Increase email length
```

#### 🛡 Add Constraint to Column
```sql
ALTER TABLE Student
MODIFY StudentID INT NOT NULL;
```

---

## 🧩 Summary

| DDL Command | Purpose                          | Example                              |
|-------------|----------------------------------|--------------------------------------|
| `CREATE`    | Define new tables or columns     | `CREATE TABLE Student (...)`         |
| `ALTER`     | Modify table or column structure | `ALTER TABLE Student ADD PhoneNumber`|
| `DROP`      | Delete tables or columns         | `DROP TABLE Address`                 |
| `TRUNCATE`  | Remove all data from a table     | `TRUNCATE TABLE Department`          |

---


## 📘 Data Manipulation Language (DML)

**DML** is a subset of SQL used to manage data in relational databases. It includes commands to **insert**, **update**, and **delete** records.

---

## 🧑‍🎓 Tables Overview

```sql
-- Student Table
CREATE TABLE Student (
    StudentID INT,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Gender CHAR(1),
    DateOfBirth DATE,
    Email VARCHAR(100),
    DepartmentID INT
);

-- Department Table
CREATE TABLE Department (
    DepartmentID INT,
    DepartmentName VARCHAR(100),
    HeadOfDepartment VARCHAR(100)
);

-- Address Table
CREATE TABLE Address (
    AddressID INT,
    StudentID INT,
    Street VARCHAR(100),
    City VARCHAR(50),
    State VARCHAR(50),
    ZipCode VARCHAR(10)
);
```

---

## ✍️ DML Examples

### 🔹 INSERT Records

#### Student Table
```sql
-- Full insert
INSERT INTO Student VALUES (101, 'Gana', 'Rao', 'M', '2003-05-12', 'gana.rao@example.com', 1);
INSERT INTO Student VALUES (102, 'Meena', 'Kumari', 'F', '2002-08-20', 'meena.k@example.com', 2);

-- Partial insert
INSERT INTO Student (StudentID, FirstName, LastName) VALUES (103, 'Raj', 'Verma');
```

#### Department Table
```sql
INSERT INTO Department VALUES (1, 'Computer Science', 'Dr. Sharma');
INSERT INTO Department VALUES (2, 'Mathematics', 'Dr. Iyer');
```

#### Address Table
```sql
INSERT INTO Address VALUES (201, 101, '123 Main St', 'Bobbili', 'AP', '535558');
INSERT INTO Address VALUES (202, 102, '456 Elm St', 'Vizianagaram', 'AP', '535002');
```

---

### ✏️ UPDATE Records

```sql
-- Safe update with WHERE
UPDATE Student SET Email = 'gana.new@example.com' WHERE StudentID = 101;

-- Risky update without WHERE
UPDATE Student SET Email = 'updated@example.com';
-- ❌ Updates email for ALL students!
```

---

### ❌ DELETE Records

```sql
-- Safe delete
DELETE FROM Address WHERE AddressID = 201;

-- Risky delete
DELETE FROM Address;
-- ❌ Deletes ALL addresses!
```

---

## 🔐 Adding Primary and Foreign Keys

### ➕ ALTER TABLE to Add Keys

```sql
-- Add Primary Keys
ALTER TABLE Student ADD PRIMARY KEY (StudentID);
ALTER TABLE Department ADD PRIMARY KEY (DepartmentID);
ALTER TABLE Address ADD PRIMARY KEY (AddressID);

-- Add Foreign Keys
ALTER TABLE Student ADD FOREIGN KEY (DepartmentID) REFERENCES Department(DepartmentID);
ALTER TABLE Address ADD FOREIGN KEY (StudentID) REFERENCES Student(StudentID);
```

---

## ⚠️ Behavior Before vs After Keys

### 🔄 Before Adding Keys

```sql
-- Insert with invalid foreign key (no error)
INSERT INTO Student VALUES (104, 'Fake', 'User', 'M', '2000-01-01', 'fake@example.com', 99);
-- Insert address for non-existent student
INSERT INTO Address VALUES (203, 999, 'Ghost St', 'Nowhere', 'NA', '000000');
```

### 🔒 After Adding Keys

```sql
-- Insert with invalid foreign key (fails)
INSERT INTO Student VALUES (105, 'Ghost', 'User', 'F', '2001-01-01', 'ghost@example.com', 99);
-- ❌ Error: DepartmentID 99 does not exist

-- Delete student with linked address (fails)
DELETE FROM Student WHERE StudentID = 101;
-- ❌ Error: Cannot delete due to foreign key constraint in Address
```

---

## 🧩 Summary

| Feature              | Before Keys           | After Keys             |
|----------------------|------------------------|-------------------------|
| Insert invalid data  | Allowed                | Blocked by constraints |
| Delete linked data   | Allowed                | Blocked by constraints |

---

## 🗑️ Deleting Data After Adding Primary and Foreign Keys

Once **Primary Key** and **Foreign Key** constraints are added to your tables, deletion operations must respect **referential integrity**. This means you cannot delete a record if it’s referenced by another table.

---

### 🔐 Constraints Recap

- **Primary Key**: Uniquely identifies each record in a table.
- **Foreign Key**: Links one table to another, enforcing valid relationships.

---

### 🧪 Example Setup

```sql
-- Add Primary Keys
ALTER TABLE Student ADD PRIMARY KEY (StudentID);
ALTER TABLE Department ADD PRIMARY KEY (DepartmentID);
ALTER TABLE Address ADD PRIMARY KEY (AddressID);

-- Add Foreign Keys
ALTER TABLE Student ADD FOREIGN KEY (DepartmentID) REFERENCES Department(DepartmentID);
ALTER TABLE Address ADD FOREIGN KEY (StudentID) REFERENCES Student(StudentID);
```

---

### ❌ Attempt to Delete a Student with Linked Address

```sql
DELETE FROM Student WHERE StudentID = 101;
-- ❌ ERROR: Cannot delete or update a parent row: a foreign key constraint fails
```

The `Address` table references `StudentID`, so deleting the student would break the relationship.

---

### 🧪 Without Cascading

If `ON DELETE CASCADE` is not defined, the same delete operation would fail:

```sql
DELETE FROM Student WHERE StudentID = 101;
-- ❌ ERROR: Cannot delete or update a parent row due to foreign key constraint
```

You would need to manually delete the child record first:

```sql
DELETE FROM Address WHERE StudentID = 101;
DELETE FROM Student WHERE StudentID = 101;
```

---

### ✅ Safe Deletion with Cascading (Optional)

If you want deletions to automatically remove related records, you can define the foreign key with `ON DELETE CASCADE`:

```sql
ALTER TABLE Address
DROP FOREIGN KEY fk_student;

ALTER TABLE Address
ADD CONSTRAINT fk_student
FOREIGN KEY (StudentID) REFERENCES Student(StudentID)
ON DELETE CASCADE;
```

Now this will work:

```sql
DELETE FROM Student WHERE StudentID = 101;
-- ✅ Automatically deletes related address records
```

---

### 🧩 Best Practices

| Action                      | Recommendation                          |
|----------------------------|------------------------------------------|
| Delete parent with children | Use `ON DELETE CASCADE` or delete child first |
| Maintain data integrity     | Always define foreign keys               |
| Avoid accidental deletion   | Use `WHERE` clause and test queries      |

---

## 🔄 Transaction Control Language (TCL)

**TCL** helps you control how changes are saved or undone in a database. It’s used when you make changes using DML commands like `INSERT`, `UPDATE`, or `DELETE`.

Think of a transaction like a “bundle of changes.” You can either save all the changes together or cancel them if something goes wrong.

---

### 🧩 Why TCL Is Important

- ✅ Keeps your data safe and consistent
- ✅ Lets you undo mistakes before saving
- ✅ Makes sure all related changes happen together
- ✅ Prevents half-done updates

---

### 🔧 Common TCL Commands

| Command     | What It Does                                  |
|-------------|-----------------------------------------------|
| `BEGIN`     | Starts a transaction                          |
| `COMMIT`    | Saves all changes made in the transaction     |
| `ROLLBACK`  | Cancels all changes made since `BEGIN`        |
| `SAVEPOINT` | Sets a point you can roll back to later       |

---

## 🧪 Simple Examples Using University Tables

### 🔹 Example 1: Insert Student and Address Together

```sql
BEGIN;

INSERT INTO Student (StudentID, FirstName, LastName, DepartmentID)
VALUES (201, 'Gana', 'Rao', 1);

INSERT INTO Address (AddressID, StudentID, Street, City, State, ZipCode)
VALUES (301, 201, '123 Main St', 'Bobbili', 'AP', '535558');

COMMIT;
-- ✅ Both records are saved together
```

### 🔹 Example 2: Rollback on Error

```sql
BEGIN;

INSERT INTO Student (StudentID, FirstName, LastName, DepartmentID)
VALUES (202, 'Meena', 'Kumari', 99);  -- ❌ DepartmentID 99 doesn't exist

INSERT INTO Address (AddressID, StudentID, Street, City, State, ZipCode)
VALUES (302, 202, '456 Elm St', 'Vizag', 'AP', '530001');

ROLLBACK;
-- ❌ All changes are canceled because of the error
```

### 🔹 Example 3: Using SAVEPOINT

```sql
BEGIN;

INSERT INTO Student VALUES (203, 'Raj', 'Verma', 1);
SAVEPOINT AfterStudent;

INSERT INTO Address VALUES (303, 203, '789 Lotus St', 'Vizianagaram', 'AP', '535002');
ROLLBACK TO AfterStudent;

COMMIT;
-- ✅ Student is saved, Address is not
```

---

## 🧩 Summary

| TCL Command | Use It When You Want To...                  |
|-------------|---------------------------------------------|
| `BEGIN`     | Start a group of changes                    |
| `COMMIT`    | Save all changes permanently                |
| `ROLLBACK`  | Undo all changes if something goes wrong    |
| `SAVEPOINT` | Undo part of the changes, not everything    |

---

## 🔐 Data Control Language (DCL)

**DCL** is used to control who can access or change data in a database. It helps database administrators manage permissions so that only authorized users can view or modify sensitive information.

---

### 🧩 Why DCL Is Important

- ✅ Protects sensitive data from unauthorized access
- ✅ Ensures only trusted users can make changes
- ✅ Helps manage user roles and responsibilities
- ✅ Supports secure collaboration in multi-user environments

---

### 🔧 Common DCL Commands

| Command   | What It Does                                 |
|-----------|----------------------------------------------|
| `GRANT`   | Gives permission to a user                   |
| `REVOKE`  | Takes away permission from a user            |

---

## 🧪 Simple Examples Using University Tables

### 🔹 Example 1: Grant SELECT Permission

```sql
GRANT SELECT ON Student TO user1;
GRANT SELECT ON Department TO user1;
GRANT SELECT ON Address TO user1;
-- ✅ user1 can now view data from these tables
```

### 🔹 Example 2: Grant INSERT and UPDATE Permission

```sql
GRANT INSERT, UPDATE ON Student TO user2;
-- ✅ user2 can add and modify student records
```

### 🔹 Example 3: Revoke Permission

```sql
REVOKE SELECT ON Address FROM user1;
-- ❌ user1 can no longer view address data
```

---

## 🧩 Summary

| DCL Command | Use It When You Want To...                     |
|-------------|------------------------------------------------|
| `GRANT`     | Allow a user to view or change data            |
| `REVOKE`    | Remove a user’s access to certain data         |
| Benefit     | Keeps your database secure and well-managed    |

---

# 🧮 SQL Aggregate Functions

Aggregate functions perform calculations on multiple rows of a table and return a single value. They are commonly used with `GROUP BY` to summarize data.

## 📌 List of Aggregate Functions

| Function     | Description                                      |
|--------------|--------------------------------------------------|
| `COUNT()`    | Returns the number of rows                       |
| `SUM()`      | Returns the total sum of a numeric column        |
| `AVG()`      | Returns the average value of a numeric column    |
| `MIN()`      | Returns the minimum value                        |
| `MAX()`      | Returns the maximum value                        |
| `STDDEV()`   | Returns the standard deviation                   |
| `VARIANCE()` | Returns the variance                             |
| `LISTAGG()`  | Concatenates values into a single string         |

---

## 🧾 Examples Without `GROUP BY`

These examples return a single result for the entire table.

```sql
-- Count total employees
SELECT COUNT(*) FROM employees;

-- Sum of all salaries
SELECT SUM(salary) FROM employees;

-- Average salary
SELECT AVG(salary) FROM employees;

-- Minimum salary
SELECT MIN(salary) FROM employees;

-- Maximum salary
SELECT MAX(salary) FROM employees;

-- Standard deviation of salaries
SELECT STDDEV(salary) FROM employees;

-- Variance of salaries
SELECT VARIANCE(salary) FROM employees;

-- Concatenate all employee names
SELECT LISTAGG(first_name, ', ') WITHIN GROUP (ORDER BY first_name) AS all_names
FROM employees;
```

---

## 📊 Examples With `GROUP BY`

These examples return one result per group.

```sql
-- Count employees per department
SELECT department_id, COUNT(*) AS employee_count
FROM employees
GROUP BY department_id;

-- Total salary per department
SELECT department_id, SUM(salary) AS total_salary
FROM employees
GROUP BY department_id;

-- Average salary per job title
SELECT job_id, AVG(salary) AS avg_salary
FROM employees
GROUP BY job_id;

-- Minimum and maximum salary per department
SELECT department_id, MIN(salary) AS min_salary, MAX(salary) AS max_salary
FROM employees
GROUP BY department_id;

-- Standard deviation of salary per job
SELECT job_id, STDDEV(salary) AS salary_stddev
FROM employees
GROUP BY job_id;

-- Variance of salary per department
SELECT department_id, VARIANCE(salary) AS salary_variance
FROM employees
GROUP BY department_id;

-- List of employee names per department
SELECT department_id,
       LISTAGG(first_name, ', ') WITHIN GROUP (ORDER BY first_name) AS names
FROM employees
GROUP BY department_id;
```

---

## 🧠 Notes

- Aggregate functions ignore `NULL` values except for `COUNT(*)`.
- You can use `HAVING` to filter groups after aggregation.

---