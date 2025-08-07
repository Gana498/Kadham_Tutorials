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

Let me know if you'd like to include rollback examples or transaction control for safer deletions!