# Experiment 2: DDL Commands

## AIM
To study and implement DDL commands and different types of constraints.

## THEORY

### 1. CREATE
Used to create a new relation (table).

**Syntax:**
```sql
CREATE TABLE (
  field_1 data_type(size),
  field_2 data_type(size),
  ...
);
```
### 2. ALTER
Used to add, modify, drop, or rename fields in an existing relation.
(a) ADD
```sql
ALTER TABLE std ADD (Address CHAR(10));
```
(b) MODIFY
```sql
ALTER TABLE relation_name MODIFY (field_1 new_data_type(size));
```
(c) DROP
```sql
ALTER TABLE relation_name DROP COLUMN field_name;
```
(d) RENAME
```sql
ALTER TABLE relation_name RENAME COLUMN old_field_name TO new_field_name;
```
### 3. DROP TABLE
Used to permanently delete the structure and data of a table.
```sql
DROP TABLE relation_name;
```
### 4. RENAME
Used to rename an existing database object.
```sql
RENAME TABLE old_relation_name TO new_relation_name;
```
### CONSTRAINTS
Constraints are used to specify rules for the data in a table. If there is any violation between the constraint and the data action, the action is aborted by the constraint. It can be specified when the table is created (using CREATE TABLE) or after it is created (using ALTER TABLE).
### 1. NOT NULL
When a column is defined as NOT NULL, it becomes mandatory to enter a value in that column.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) NOT NULL
);
```
### 2. UNIQUE
Ensures that values in a column are unique.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) UNIQUE
);
```
### 3. CHECK
Specifies a condition that each row must satisfy.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) CHECK (logical_expression)
);
```
### 4. PRIMARY KEY
Used to uniquely identify each record in a table.
Properties:
Must contain unique values.
Cannot be null.
Should contain minimal fields.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) PRIMARY KEY
);
```
### 5. FOREIGN KEY
Used to reference the primary key of another table.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size),
  FOREIGN KEY (column_name) REFERENCES other_table(column)
);
```
### 6. DEFAULT
Used to insert a default value into a column if no value is specified.

Syntax:
```sql
CREATE TABLE Table_Name (
  col_name1 data_type,
  col_name2 data_type,
  col_name3 data_type DEFAULT 'default_value'
);
```

**Question 1**
--
-- Paste Question 1 here
Create a table named Employees with the following constraints:

EmployeeID should be the primary key.
FirstName and LastName should be NOT NULL.
Email should be unique.
Salary should be greater than 0.
DepartmentID should be a foreign key referencing the Departments table.

```sql
-- Paste your SQL code below for Question 1
CREATE TABLE Employees (
    EmployeeID   INTEGER PRIMARY KEY,
    FirstName    TEXT NOT NULL,
    LastName     TEXT NOT NULL,
    Email        TEXT UNIQUE,
    Salary       REAL CHECK (Salary > 0),
    DepartmentID INTEGER,
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
);
```

**Output:**

<img width="1920" height="1080" alt="q1" src="https://github.com/user-attachments/assets/d433c8bb-7dc3-4611-bb35-fe1b9b90a6ff" />

**Question 2**
---
-- Paste Question 2 here
Insert all products from Discontinued_products into Products.

Table attributes are ProductID, ProductName, Price, Stock

```sql
-- Paste your SQL code below for Question 2
INSERT INTO Products (ProductID, ProductName, Price, Stock)
SELECT ProductID, ProductName, Price, Stock
FROM Discontinued_products;

```

**Output:**

<img width="1920" height="1080" alt="q2" src="https://github.com/user-attachments/assets/4bb38f46-f177-4154-a934-5cca83493d09" />


**Question 3**
---
-- Paste Question 3 here
Create a new table named orders with the following specifications:
ord_id as TEXT with a length of 4.
item_id as TEXT.
ord_date as DATE.
ord_qty as INTEGER.
cost as INTEGER.
The primary key is a composite key consisting of item_id and ord_date.
ord_id and item_id should not accept NULL
```sql
-- Paste your SQL code below for Question 3
CREATE TABLE orders (
    ord_id TEXT NOT NULL CHECK (length(ord_id) = 4 ),
    item_id TEXT NOT NULL ,
    ord_date DATE ,
    ord_qty INTEGER ,
    cost INTEGER ,
    PRIMARY KEY (item_id , ord_date) 
);
```

**Output:**

<img width="1920" height="1080" alt="q3" src="https://github.com/user-attachments/assets/0aeefe0d-33ce-4740-ad84-e88ffdb6c6e6" />


**Question 4**
---
-- Paste Question 4 here
Insert a customer with CustomerID 301, Name Michael Jordan, Address 123 Maple St, City Chicago, and ZipCode 60616 into the Customers table.
```sql
-- Paste your SQL code below for Question 4
INSERT INTO Customers (CustomerID ,Name, Address, City, ZipCode)
VALUES (301, 'Michael Jordan', '123 Maple St', 'Chicago', 60616);
```

**Output:**

<img width="767" height="163" alt="Q4" src="https://github.com/user-attachments/assets/8d6541d9-5c28-4087-a7c7-b33ba3e08d42" />


**Question 5**
---
-- Paste Question 5 here
Write an SQL query to add two new columns, first_name and last_name, to the table employee. Both columns should have a data type of varchar(50).
```sql
-- Paste your SQL code below for Question 5
ALTER TABLE employee
ADD COLUMN first_name varchar(50);

ALTER TABLE employee
ADD COLUMN last_name varchar(50);

```

**Output:**

<img width="863" height="256" alt="q5" src="https://github.com/user-attachments/assets/fdc3e149-7a3f-4bf9-9e2d-9aaacba9c05b" />


**Question 6**
---
-- Paste Question 6 here
Create a table named Locations with the following columns:

LocationID as INTEGER
LocationName as TEXT
Address as TEXT
```sql
-- Paste your SQL code below for Question 6
CREATE TABLE Locations (
    LocationID   INTEGER,
    LocationName TEXT,
    Address      TEXT
);

```

**Output:**

<img width="891" height="319" alt="q6" src="https://github.com/user-attachments/assets/3eb7323e-4592-4759-ba64-33ad4ff61b1e" />


**Question 7**
---
-- Paste Question 7 here
Write a SQL query to add a new column MobileNumber of type NUMBER and a new column Address of type VARCHAR(100) to the Student_details table.
```sql
-- Paste your SQL code below for Question 7
ALTER TABLE Student_details
ADD COLUMN MobileNumber NUMBER;

ALTER TABLE Student_details
ADD COLUMN Address VARCHAR(100);

```

**Output:**

<img width="862" height="355" alt="q7" src="https://github.com/user-attachments/assets/a0ccb3ed-4120-4dc9-88d9-b82519728769" />


**Question 8**
---
-- Paste Question 8 here
Create a table named Attendance with the following constraints:
AttendanceID as INTEGER should be the primary key.
EmployeeID as INTEGER should be a foreign key referencing Employees(EmployeeID).
AttendanceDate as DATE.
Status as TEXT should be one of 'Present', 'Absent', 'Leave'.
```sql
-- Paste your SQL code below for Question 8
CREATE TABLE Attendance (
    AttendanceID   INTEGER PRIMARY KEY,
    EmployeeID     INTEGER,
    AttendanceDate DATE,
    Status         TEXT CHECK (Status IN ('Present', 'Absent', 'Leave')),
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
);

```

**Output:**

<img width="693" height="254" alt="q8" src="https://github.com/user-attachments/assets/903c6dbb-b9c3-4b63-89af-88f103129baa" />

**Question 9**
---
-- Paste Question 9 here
In the Student_details table, insert a student record where some fields are NULL, another record where all fields are filled without any NULL values, and a third record where some fields are filled, and others are left as NULL.

RollNo      Name            Gender      Subject      MARKS
----------  ------------    ----------  ----------   ----------
205         Olivia Green    F
207         Liam Smith      M           Mathematics  85
208         Sophia Johnson  F           Science
```sql
-- Paste your SQL code below for Question 9
INSERT INTO Student_details (RollNo, Name, Gender)
VALUES (205, 'Olivia Green', 'F');

INSERT INTO Student_details (RollNo, Name, Gender, Subject, MARKS)
VALUES (207, 'Liam Smith', 'M', 'Mathematics', 85);

INSERT INTO Student_details (RollNo, Name, Gender, Subject)
VALUES (208, 'Sophia Johnson', 'F', 'Science');

```

**Output:**

<img width="787" height="265" alt="q9" src="https://github.com/user-attachments/assets/ec5129fe-5ad8-453f-9e9d-2328af67c40f" />


## RESULT
Thus, the SQL queries to implement different types of constraints and DDL commands have been executed successfully.
