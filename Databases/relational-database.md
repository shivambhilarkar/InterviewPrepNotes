# 📘 Relational Database Concepts

This README provides a comprehensive overview of core relational database elements: **Tables**, **Primary Keys**, **Foreign Keys**, **Types of Keys**, and **Schema**.

---

## 🧱 Tables

A **table** is the fundamental unit of data storage in a relational database. It organizes data into rows and columns:

- **Rows (Records):** Each row represents a unique data item.
- **Columns (Fields):** Each column represents a property or attribute of the data.

### 📋 Example

| EmployeeID | Name    | Department | Salary |
| ---------- | ------- | ---------- | ------ |
| 1          | Alice   | HR         | 50000  |
| 2          | Bob     | IT         | 60000  |
| 3          | Charlie | Finance    | 55000  |

---

## 🔑 Primary Key

A **Primary Key** uniquely identifies each record in a table.

### ✅ Characteristics:

- Must be **unique** across rows.
- Cannot contain **NULL** values.
- Only one primary key is allowed per table.
- Can be **single-column** or **composite** (multi-column).

### 🛠️ Example

```sql
CREATE TABLE Employee (
  EmployeeID INT PRIMARY KEY,
  Name VARCHAR(100),
  Department VARCHAR(100),
  Salary DECIMAL(10,2)
);
```

Here, `EmployeeID` is the primary key.

---

## 🔗 Foreign Key

A **Foreign Key** is a column that creates a relationship between two tables. It refers to the **Primary Key** of another table.

### 🔄 Characteristics:

- Maintains **referential integrity**.
- Can accept **NULLs** (if optional relationship).
- Enforces constraints like **ON DELETE CASCADE**, etc.

### 🛠️ Example

```sql
CREATE TABLE Department (
  DeptID INT PRIMARY KEY,
  DeptName VARCHAR(100)
);

CREATE TABLE Employee (
  EmployeeID INT PRIMARY KEY,
  Name VARCHAR(100),
  DeptID INT,
  FOREIGN KEY (DeptID) REFERENCES Department(DeptID)
);
```

Here, `DeptID` in the `Employee` table is a **foreign key** referencing `DeptID` in the `Department` table.

---

## 🗝️ Types of Keys

| Key Type          | Description                                                                |
| ----------------- | -------------------------------------------------------------------------- |
| **Primary Key**   | Uniquely identifies each row. Only one per table.                          |
| **Foreign Key**   | Refers to a primary key in another table.                                  |
| **Candidate Key** | A column, or set of columns, that can qualify as a primary key.            |
| **Composite Key** | A primary key made up of two or more columns.                              |
| **Alternate Key** | A candidate key that is not selected as the primary key.                   |
| **Super Key**     | Any combination of columns that uniquely identifies a row (includes PKs).  |
| **Unique Key**    | Ensures all values in a column (or group) are unique, but allows one NULL. |

---

## 🧬 Schema

A **Schema** is the blueprint or logical structure of a database. It defines:

- Tables
- Columns and their data types
- Relationships between tables (e.g., foreign keys)
- Constraints (e.g., NOT NULL, UNIQUE)
- Indexes
- Views
- Stored procedures and functions (optional)

### 🧾 Example Schema

```sql
-- Customers table
CREATE TABLE Customer (
  CustomerID INT PRIMARY KEY,
  Name VARCHAR(100),
  Email VARCHAR(100) UNIQUE
);

-- Orders table
CREATE TABLE Orders (
  OrderID INT PRIMARY KEY,
  OrderDate DATE,
  CustomerID INT,
  FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
);
```

In this schema:

- The `Customer` table holds customer data.
- The `Orders` table references `CustomerID`, creating a **one-to-many** relationship between customers and their orders.

---

## 📚 Summary

- **Table**: Stores data in rows and columns.
- **Primary Key**: Uniquely identifies rows in a table.
- **Foreign Key**: Links records between tables.
- **Keys**: Help enforce data integrity and relationships.
- **Schema**: Defines the structure and organization of the database.

---

---

# 🔗 SQL Joins Explained

This document provides a detailed explanation of various types of SQL joins used in relational databases, including **Inner Join**, **Left Join**, **Right Join**, **Full Outer Join**, and **Self Join**.

---

## 📘 What is a Join?

A **JOIN** clause in SQL is used to combine rows from two or more tables based on a related column between them, typically a **foreign key**.

---

## 🟡 1. INNER JOIN

Returns **only the matching rows** between the two tables based on the join condition.

### 📌 Syntax

```sql
SELECT *
FROM TableA
INNER JOIN TableB
ON TableA.common_column = TableB.common_column;
```

### 📋 Example

**Customers**

| CustomerID | Name  |
| ---------- | ----- |
| 1          | Alice |
| 2          | Bob   |

**Orders**

| OrderID | CustomerID | Product  |
| ------- | ---------- | -------- |
| 101     | 1          | Laptop   |
| 102     | 2          | Keyboard |

**Result (INNER JOIN on CustomerID):**

| CustomerID | Name  | OrderID | Product  |
| ---------- | ----- | ------- | -------- |
| 1          | Alice | 101     | Laptop   |
| 2          | Bob   | 102     | Keyboard |

---

## 🟢 2. LEFT JOIN (or LEFT OUTER JOIN)

Returns **all rows from the left table**, and the **matched rows** from the right table. If there is no match, NULLs are returned for right table columns.

### 📌 Syntax

```sql
SELECT *
FROM TableA
LEFT JOIN TableB
ON TableA.common_column = TableB.common_column;
```

### 📋 Example (add a customer with no orders):

**Customers**

| CustomerID | Name    |
| ---------- | ------- |
| 1          | Alice   |
| 2          | Bob     |
| 3          | Charlie |

**Result (LEFT JOIN on CustomerID):**

| CustomerID | Name    | OrderID | Product  |
| ---------- | ------- | ------- | -------- |
| 1          | Alice   | 101     | Laptop   |
| 2          | Bob     | 102     | Keyboard |
| 3          | Charlie | NULL    | NULL     |

---

## 🔵 3. RIGHT JOIN (or RIGHT OUTER JOIN)

Returns **all rows from the right table**, and the **matched rows** from the left table. If there is no match, NULLs are returned for left table columns.

### 📌 Syntax

```sql
SELECT *
FROM TableA
RIGHT JOIN TableB
ON TableA.common_column = TableB.common_column;
```

### 📋 Example (add an order with unknown customer):

**Orders**

| OrderID | CustomerID | Product  |
| ------- | ---------- | -------- |
| 101     | 1          | Laptop   |
| 102     | 2          | Keyboard |
| 103     | 4          | Mouse    |

**Result (RIGHT JOIN on CustomerID):**

| CustomerID | Name  | OrderID | Product  |
| ---------- | ----- | ------- | -------- |
| 1          | Alice | 101     | Laptop   |
| 2          | Bob   | 102     | Keyboard |
| 4          | NULL  | 103     | Mouse    |

---

## 🟣 4. FULL OUTER JOIN

Returns **all rows from both tables**. Where there is no match, NULLs are filled in for missing values.

### 📌 Syntax

```sql
SELECT *
FROM TableA
FULL OUTER JOIN TableB
ON TableA.common_column = TableB.common_column;
```

> ⚠️ Note: Not supported in some databases like MySQL without workarounds.

### 📋 Result (Full Outer Join on CustomerID):

| CustomerID | Name    | OrderID | Product  |
| ---------- | ------- | ------- | -------- |
| 1          | Alice   | 101     | Laptop   |
| 2          | Bob     | 102     | Keyboard |
| 3          | Charlie | NULL    | NULL     |
| 4          | NULL    | 103     | Mouse    |

---

## 🔁 5. SELF JOIN

A **Self Join** is a regular join where a table is joined with itself.

### 📌 Syntax

```sql
SELECT A.EmployeeID, A.Name, B.Name AS Manager
FROM Employee A
JOIN Employee B ON A.ManagerID = B.EmployeeID;
```

### 📋 Example

**Employee**

| EmployeeID | Name    | ManagerID |
| ---------- | ------- | --------- |
| 1          | Alice   | NULL      |
| 2          | Bob     | 1         |
| 3          | Charlie | 1         |

**Result (Self Join):**

| EmployeeID | Name    | Manager |
| ---------- | ------- | ------- |
| 2          | Bob     | Alice   |
| 3          | Charlie | Alice   |

---

## 📚 Summary Table

| Join Type       | Description                                      |
| --------------- | ------------------------------------------------ |
| INNER JOIN      | Only matched records from both tables            |
| LEFT JOIN       | All records from left table + matched from right |
| RIGHT JOIN      | All records from right table + matched from left |
| FULL OUTER JOIN | All records from both tables                     |
| SELF JOIN       | Join a table with itself                         |

---

## ✅ Best Practices

- Use `INNER JOIN` when you only want intersecting records.
- Use `LEFT JOIN` to preserve all data from the primary (left) table.
- Avoid `FULL OUTER JOIN` in MySQL unless absolutely necessary (requires union workaround).
- Always use `table aliases` for better readability in multi-table joins.
- Ensure indexes on join keys for better performance.

---

---

# 🧠 Database Normalization Explained

**Normalization** is the process of organizing data in a database to reduce redundancy and improve data integrity.

---

## 🎯 Objectives of Normalization

- Eliminate redundant (duplicate) data.
- Ensure data dependencies make sense (i.e., data is logically stored).
- Avoid anomalies (update, insert, delete).
- Improve data consistency and efficiency.

---

## ✅ Normal Forms Overview

| Normal Form | Requirement                                               |
| ----------- | --------------------------------------------------------- |
| 1NF         | Atomic columns, no repeating groups                       |
| 2NF         | Be in 1NF + no partial dependency                         |
| 3NF         | Be in 2NF + no transitive dependency                      |
| BCNF        | A stronger version of 3NF                                 |
| 4NF         | Be in BCNF + no multi-valued dependencies                 |
| 5NF         | Be in 4NF + no join dependency (project-join normal form) |

---

## 🔹 First Normal Form (1NF)

**Rule:**

- Each column should have atomic (indivisible) values.
- Each row should be unique.
- No repeating groups or arrays.

### ❌ Non-1NF Example

| StudentID | Name  | Courses       |
| --------- | ----- | ------------- |
| 1         | Alice | Math, English |
| 2         | Bob   | Science       |

### ✅ 1NF Example

| StudentID | Name  | Course  |
| --------- | ----- | ------- |
| 1         | Alice | Math    |
| 1         | Alice | English |
| 2         | Bob   | Science |

---

## 🔸 Second Normal Form (2NF)

**Rule:**

- Be in 1NF.
- No **partial dependency**: A non-key attribute should depend on the **entire** primary key (especially in composite keys).

### ❌ Non-2NF Example

```sql
-- Composite key: (StudentID, Course)
| StudentID | Course   | StudentName |
|-----------|----------|-------------|
| 1         | Math     | Alice       |
| 1         | English  | Alice       |
```

Here, `StudentName` depends only on `StudentID`, not the full primary key.

### ✅ 2NF Solution

Split into two tables:

**Students**

| StudentID | StudentName |
| --------- | ----------- |
| 1         | Alice       |

**Enrollments**

| StudentID | Course  |
| --------- | ------- |
| 1         | Math    |
| 1         | English |

---

## 🔹 Third Normal Form (3NF)

**Rule:**

- Be in 2NF.
- No **transitive dependencies** (non-key column depends on another non-key column).

### ❌ Non-3NF Example

| EmployeeID | Name  | Department | DeptLocation |
| ---------- | ----- | ---------- | ------------ |
| 1          | Alice | HR         | 1st Floor    |
| 2          | Bob   | IT         | 2nd Floor    |

Here, `DeptLocation` depends on `Department`, not directly on the primary key.

### ✅ 3NF Solution

**Employees**

| EmployeeID | Name  | Department |
| ---------- | ----- | ---------- |
| 1          | Alice | HR         |
| 2          | Bob   | IT         |

**Departments**

| Department | DeptLocation |
| ---------- | ------------ |
| HR         | 1st Floor    |
| IT         | 2nd Floor    |

---

## 🔸 Boyce-Codd Normal Form (BCNF)

**Rule:**

- Be in 3NF.
- For every functional dependency `X → Y`, X should be a super key.

> Stronger than 3NF. All candidate keys must satisfy dependency rules.

### ❌ BCNF Violation Example

| StudentID | Course | Instructor |
| --------- | ------ | ---------- |
| 1         | Math   | Prof. A    |
| 2         | Math   | Prof. A    |

Here, `Course → Instructor`, but `Course` is not a key.

### ✅ BCNF Solution

**Courses**

| Course | Instructor |
| ------ | ---------- |
| Math   | Prof. A    |

**Enrollments**

| StudentID | Course |
| --------- | ------ |
| 1         | Math   |
| 2         | Math   |

---

## 🔹 Fourth Normal Form (4NF)

**Rule:**

- Be in BCNF.
- No multi-valued dependencies (MVDs).

### ❌ 4NF Violation Example

| StudentID | Language | Hobby |
| --------- | -------- | ----- |
| 1         | English  | Chess |
| 1         | English  | Music |
| 1         | French   | Chess |
| 1         | French   | Music |

MVD exists: one student has multiple languages and multiple hobbies independently.

### ✅ 4NF Solution

**StudentLanguages**

| StudentID | Language |
| --------- | -------- |
| 1         | English  |
| 1         | French   |

**StudentHobbies**

| StudentID | Hobby |
| --------- | ----- |
| 1         | Chess |
| 1         | Music |

---

## 🔸 Fifth Normal Form (5NF)

**Rule:**

- Be in 4NF.
- No join dependencies (i.e., data cannot be reconstructed using just projections).

> Extremely rare in practice.

### Example:

Used when decomposing data into multiple tables is necessary to avoid redundancy but can't be recombined without losing information unless 5NF is applied.

---

## 🧩 Summary

| Normal Form | Main Requirement                   |
| ----------- | ---------------------------------- |
| 1NF         | Atomic values, no repeating groups |
| 2NF         | No partial dependency              |
| 3NF         | No transitive dependency           |
| BCNF        | Every determinant is a super key   |
| 4NF         | No multi-valued dependencies       |
| 5NF         | No join dependencies               |

---

## ✅ Best Practices

- Normalize up to 3NF for most real-world applications.
- Use BCNF or higher for academic or very complex use cases.
- De-normalize when performance is more critical than redundancy.

---

# 📦 Denormalization in Relational Databases

**Denormalization** is the process of combining normalized tables into larger tables to improve database read performance at the cost of some redundancy and potential anomalies.

---

## 🎯 Why Denormalize?

- Improve **read performance** by reducing JOINs.
- Optimize **reporting queries**.
- Minimize **complex queries**.
- Simplify data access patterns in read-heavy applications.

---

## ⚖️ Normalization vs. Denormalization

| Feature     | Normalization                   | Denormalization               |
| ----------- | ------------------------------- | ----------------------------- |
| Redundancy  | Eliminated or minimized         | Introduced intentionally      |
| Performance | Slower reads, faster writes     | Faster reads, slower writes   |
| Complexity  | More tables, more JOINs         | Fewer tables, fewer JOINs     |
| Integrity   | High (less chance of anomalies) | Lower (needs more safeguards) |

---

# ACID properties in relational database.

# 🧪 ACID Properties in Databases

The **ACID** properties are a set of principles that ensure reliable and consistent processing of database transactions. These properties are crucial for maintaining data integrity, especially in systems where multiple transactions occur simultaneously.

---

## 1. **Atomicity**

- **Definition**: Atomicity ensures that a transaction is treated as a single, indivisible unit. Either all operations within the transaction are executed successfully, or none of them are applied.
- **Key Points**:
  - If a transaction fails at any point, the database is rolled back to its previous state.
  - Prevents partial updates that could leave the database in an inconsistent state.
  - Ensures that the database remains consistent even in the event of a failure.
- **Example**:
  ```sql
  BEGIN TRANSACTION;
  UPDATE Account SET Balance = Balance - 100 WHERE AccountID = 1;
  UPDATE Account SET Balance = Balance + 100 WHERE AccountID = 2;
  COMMIT;
  ```
- If the second UPDATE fails (e.g., due to insufficient balance), the first UPDATE is rolled back, ensuring no money is lost or created.

## 2. **Consistency**

- **Definition**: Consistency ensures that a transaction brings the database from one valid state to another, maintaining all predefined rules, constraints, and relationships.
- **Key Points**:
- Enforces data integrity constraints such as primary keys, foreign keys, unique constraints, and check constraints.
- Prevents invalid data from being written to the database.
- Ensures that the database adheres to its schema and business rules at all times.
- **Example**:
- If a transaction attempts to insert a duplicate value into a column with a UNIQUE constraint, the transaction will fail, preserving consistency.

```sql
INSERT INTO Users (UserID, Email) VALUES (1, 'example@example.com');
INSERT INTO Users (UserID, Email) VALUES (1, 'duplicate@example.com'); -- This will fail due to UNIQUE constraint.
```

## 3. **Isolation**

- **Definition**: Isolation ensures that concurrent transactions do not interfere with each other. Each transaction is executed as if it were the only transaction in the system.

- **Key Points**:
- Prevents issues like dirty reads, non-repeatable reads, and phantom reads.
- Isolation levels define the degree of isolation between transactions:
  - READ UNCOMMITTED: Transactions can read uncommitted changes (least isolation).
  - READ COMMITTED: Transactions can only read committed changes.
  - REPEATABLE READ: Ensures that data read during a transaction cannot change.
  - SERIALIZABLE: Transactions are executed sequentially (highest isolation).

**Example**:

- If two transactions try to update the same record simultaneously, isolation ensures that one transaction completes before the other begins.

```sql
-- Transaction 1
BEGIN TRANSACTION;
UPDATE Inventory SET Quantity = Quantity - 1 WHERE ProductID = 101;

-- Transaction 2
BEGIN TRANSACTION;
UPDATE Inventory SET Quantity = Quantity - 1 WHERE ProductID = 101;
```

- Depending on the isolation level, Transaction 2 will either wait for Transaction 1 to complete or fail.

## 4. **Durability**

- **Definition**: Durability ensures that once a transaction is committed, its changes are permanent, even in the event of a system failure.
- **Key Points**:
- Uses mechanisms like write-ahead logging and checkpoints to guarantee durability.
- Ensures that committed data is not lost due to power outages, crashes, or other failures.
- Relies on persistent storage (e.g., hard drives, SSDs) to store committed changes.

**Example**:

- After a COMMIT, the changes are written to disk, ensuring they persist even if the database server crashes.

```sql
BEGIN TRANSACTION;
UPDATE Orders SET Status = 'Completed' WHERE OrderID = 123;
COMMIT; -- Changes are now permanent and will survive a crash.
```

---

## 📚 Summary of ACID Properties

| Property    | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| Atomicity   | Ensures all-or-nothing execution of transactions.            |
| Consistency | Guarantees that the database remains in a valid state.       |
| Isolation   | Prevents interference between concurrent transactions.       |
| Durability  | Ensures committed transactions persist even after a failure. |


---
