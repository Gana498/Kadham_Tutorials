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

