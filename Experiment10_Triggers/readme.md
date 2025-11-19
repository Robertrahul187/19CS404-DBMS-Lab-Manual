# Experiment 10: PL/SQL – Triggers

## AIM
To write and execute PL/SQL trigger programs for automating actions in response to specific table events like INSERT, UPDATE, or DELETE.

---

## THEORY

A **trigger** is a stored PL/SQL block that is automatically executed or fired when a specified event occurs on a table or view. Triggers can be used for enforcing business rules, auditing changes, or automatic updates.

### Types of Triggers:
- **Before Trigger**: Executes before the operation (INSERT, UPDATE, DELETE).
- **After Trigger**: Executes after the operation.
- **Row-level Trigger**: Executes for each affected row.
- **Statement-level Trigger**: Executes once for the triggering statement.

**Basic Syntax:**
```sql
CREATE OR REPLACE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE ON table_name
[FOR EACH ROW]
BEGIN
   -- trigger logic
END;
```

## 1. Write a trigger to log every insertion into a table.
**Steps:**
- Create two tables: `employees` (for storing data) and `employee_log` (for logging the inserts).
- Write an **AFTER INSERT** trigger on the `employees` table to log the new data into the `employee_log` table.

```sql
-- Step 1: Create employees table
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    salary NUMBER,
    department VARCHAR2(50)
);

-- Step 2: Create employee_log table for logging
CREATE TABLE employee_log (
    log_id NUMBER PRIMARY KEY,
    emp_id NUMBER,
    action VARCHAR2(20),
    action_date DATE,
    emp_name VARCHAR2(50),
    salary NUMBER
);

-- Create sequence for log_id
CREATE SEQUENCE log_seq START WITH 1 INCREMENT BY 1;

-- Step 3: Create AFTER INSERT trigger
CREATE OR REPLACE TRIGGER log_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_log (log_id, emp_id, action, action_date, emp_name, salary)
    VALUES (log_seq.NEXTVAL, :NEW.emp_id, 'INSERT', SYSDATE, :NEW.emp_name, :NEW.salary);
    
    DBMS_OUTPUT.PUT_LINE('Insert logged for employee: ' || :NEW.emp_name);
END;
/

-- Test the trigger
BEGIN
    INSERT INTO employees VALUES (101, 'John Doe', 5000, 'IT');
    INSERT INTO employees VALUES (102, 'Jane Smith', 6000, 'HR');
    COMMIT;
END;
/

-- Verify results
SELECT * FROM employees;
SELECT * FROM employee_log;
```

**Expected Output:**
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.

<img width="1198" height="337" alt="image" src="https://github.com/user-attachments/assets/b473f8f7-ec73-4efc-9eb0-351b116cc0a7" />

---

## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.

```sql
-- Step 1: Create employees table
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    salary NUMBER,
    department VARCHAR2(50)
);

-- Step 2: Create employee_log table for logging
CREATE TABLE employee_log (
    log_id NUMBER PRIMARY KEY,
    emp_id NUMBER,
    action VARCHAR2(20),
    action_date DATE,
    emp_name VARCHAR2(50),
    salary NUMBER
);

-- Create sequence for log_id
CREATE SEQUENCE log_seq START WITH 1 INCREMENT BY 1;

-- Step 3: Create AFTER INSERT trigger
CREATE OR REPLACE TRIGGER log_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_log (log_id, emp_id, action, action_date, emp_name, salary)
    VALUES (log_seq.NEXTVAL, :NEW.emp_id, 'INSERT', SYSDATE, :NEW.emp_name, :NEW.salary);
    
    DBMS_OUTPUT.PUT_LINE('Insert logged for employee: ' || :NEW.emp_name);
END;
/

-- Test the trigger
BEGIN
    INSERT INTO employees VALUES (101, 'John Doe', 5000, 'IT');
    INSERT INTO employees VALUES (102, 'Jane Smith', 6000, 'HR');
    COMMIT;
END;
/

-- Verify results
SELECT * FROM employees;
SELECT * FROM employee_log;
```

**Expected Output:**
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`

<img width="1194" height="336" alt="image" src="https://github.com/user-attachments/assets/8c7ae1ff-ffa2-4512-a9b5-6db841dba036" />

---

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.

```sql
CREATE TABLE products (
    product_id NUMBER PRIMARY KEY,
    product_name VARCHAR2(50),
    price NUMBER,
    last_modified DATE
);

CREATE OR REPLACE TRIGGER trg_update_last_modified
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    :NEW.last_modified := SYSDATE;
END;
/

INSERT INTO products (product_id, product_name, price) VALUES (1, 'Product1', 100);

UPDATE products SET price = 120 WHERE product_id = 1;

SELECT product_id, product_name, price, last_modified FROM products WHERE product_id = 1;
```

**Expected Output:**
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.

<img width="775" height="212" alt="image" src="https://github.com/user-attachments/assets/b89338f9-5ba2-42d8-a456-399c3f407885" />

---

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.

```sql
CREATE TABLE audit_log (
    table_name VARCHAR2(50) PRIMARY KEY,
    update_count NUMBER
);

CREATE TABLE customer_orders (
    order_id NUMBER PRIMARY KEY,
    customer_name VARCHAR2(50),
    order_amount NUMBER
);

INSERT INTO audit_log (table_name, update_count) VALUES ('CUSTOMER_ORDERS', 0);

CREATE OR REPLACE TRIGGER trg_after_update_customer_orders
AFTER UPDATE ON customer_orders
DECLARE
BEGIN
    UPDATE audit_log
    SET update_count = update_count + 1
    WHERE table_name = 'CUSTOMER_ORDERS';
END;
/

INSERT INTO customer_orders VALUES (1, 'Alice', 100);
UPDATE customer_orders SET order_amount = 150 WHERE order_id = 1;
SELECT * FROM audit_log WHERE table_name = 'CUSTOMER_ORDERS';
```

**Expected Output:**
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.

<img width="787" height="263" alt="image" src="https://github.com/user-attachments/assets/bb12891e-6caa-486f-b412-edae83b303c6" />

---

## 5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.

```sql
CREATE OR REPLACE TRIGGER trg_check_salary_before_insert
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF :NEW.salary < 3000 THEN
        RAISE_APPLICATION_ERROR(-20001, 'ERROR: Salary below minimum threshold.');
    END IF;
END;
/
INSERT INTO employees (emp_id, emp_name, salary) VALUES (101, 'John Doe', 2500);
INSERT INTO employees (emp_id, emp_name, salary) VALUES (102, 'Jane Smith', 3500);
```

**Expected Output:**
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`

<img width="802" height="155" alt="image" src="https://github.com/user-attachments/assets/9eb23bf9-d12c-486d-914f-26c4889d7033" />


## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.

