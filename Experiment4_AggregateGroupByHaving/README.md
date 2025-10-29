# Experiment 4: Aggregate Functions, Group By and Having Clause

## AIM
To study and implement aggregate functions, GROUP BY, and HAVING clause with suitable examples.

## THEORY

### Aggregate Functions
These perform calculations on a set of values and return a single value.

- **MIN()** – Smallest value  
- **MAX()** – Largest value  
- **COUNT()** – Number of rows  
- **SUM()** – Total of values  
- **AVG()** – Average of values

**Syntax:**
```sql
SELECT AGG_FUNC(column_name) FROM table_name WHERE condition;
```
### GROUP BY
Groups records with the same values in specified columns.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name;
```
### HAVING
Filters the grouped records based on aggregate conditions.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

**Question 1**
--
-- Paste Question 1 here
Write a SQL Query to find how many medications are prescribed for each patient?

Sample table:MedicalRecords Table



For example:

Result
PatientID   AvgMedications
----------  --------------
4           5
6           1
7           1
8           3

```sql
-- Paste your SQL code below for Question 1
SELECT 
    PatientID, 
    COUNT(*) AS AvgMedications
FROM 
    MedicalRecords
GROUP BY 
    PatientID;

```

**Output:**

<img width="845" height="780" alt="image" src="https://github.com/user-attachments/assets/7e5628bd-4f4f-48d3-9338-52f7f9ee94ee" />

**Question 2**
---
-- Paste Question 2 here
What is the most common diagnosis among patients?

Sample table:MedicalRecords Table



For example:

Result
Diagnosis              DiagnosisCount
---------------------  --------------
Childhood vaccination  3
```sql
-- Paste your SQL code below for Question 2
SELECT Diagnosis, COUNT(*) AS DiagnosisCount
FROM MedicalRecords
GROUP BY Diagnosis
ORDER BY DiagnosisCount DESC
LIMIT 1;

```

**Output:**

<img width="1101" height="453" alt="image" src="https://github.com/user-attachments/assets/4ec57651-fcaf-492f-b834-166d0d5da3ed" />

**Question 3**
---
-- Paste Question 3 here
How many appointments are scheduled for each doctor?

Sample table:Appointments Table



For example:

Result
DoctorID    TotalAppointments
----------  -----------------
3           3
4           2
6           1
7           3
10          1

```sql
-- Paste your SQL code below for Question 3
SELECT DoctorID, COUNT(*) AS TotalAppointments
FROM Appointments
GROUP BY DoctorID;
```

**Output:**

<img width="946" height="789" alt="image" src="https://github.com/user-attachments/assets/70bd3753-ff0d-4bf6-a0ed-2a09dc0cd20a" />

**Question 4**
---
-- Paste Question 4 here
Write a SQL query to find the total number of unique cities in the customer table?

Table: customer

name        type
----------  ----------
id          INTEGER
name        TEXT
city        TEXT
email       TEXT
phone       INTEGER
For example:

Result
unique_cities
-------------
10

```sql
-- Paste your SQL code below for Question 4
SELECT COUNT(DISTINCT city) AS unique_cities
FROM customer;
```

**Output:**

<img width="718" height="475" alt="image" src="https://github.com/user-attachments/assets/c48b1e28-e171-49cd-b462-db92f21335e3" />

**Question 5**
---
-- Paste Question 5 here
Write a SQL query to find the maximum purchase amount.

Sample table: orders

ord_no      purch_amt   ord_date    customer_id  salesman_id

----------  ----------  ----------  -----------  -----------

70001       150.5       2012-10-05  3005         5002

70009       270.65      2012-09-10  3001         5005

70002       65.26       2012-10-05  3002         5001

 

For example:

Result
MAXIMUM
----------
5760.0

```sql
-- Paste your SQL code below for Question 5
SELECT MAX(purch_amt) AS MAXIMUM
FROM orders;
```

**Output:**

<img width="709" height="499" alt="image" src="https://github.com/user-attachments/assets/14e03f30-6d07-46e9-83b9-884bcc2637ab" />

**Question 6**
---
-- Paste Question 6 here
Write a SQL query to determine the number of customers who received at least one grade for their activity.

Sample table: customer

customer_id |   cust_name    |    city    | grade | salesman_id 

-------------+----------------+------------+-------+-------------

        3002 | Nick Rimando   | New York   |   100 |        5001

        3007 | Brad Davis     | New York   |   200 |        5001

        3005 | Graham Zusi    | California |   200 |        5002

 

For example:

Result
COUNT
----------
8

```sql
-- Paste your SQL code below for Question 6
SELECT COUNT(*) AS COUNT
FROM customer
WHERE grade IS NOT NULL;

```

**Output:**

<img width="711" height="575" alt="image" src="https://github.com/user-attachments/assets/c9eea56a-011f-4482-a46f-d1c30ee3d7fb" />

**Question 7**
---
-- Paste Question 7 here
Write a SQL query to calculate the total number of working hours of all employees

Sample table: employee1


 

For example:

Result
Total working hours
-------------------
111

```sql
-- Paste your SQL code below for Question 7
SELECT SUM(workhour) AS "Total working hours"
FROM employee1;

```

**Output:**

<img width="900" height="436" alt="image" src="https://github.com/user-attachments/assets/b2e842d8-8097-4252-bc7a-d70e7a838f70" />

**Question 8**
---
-- Paste Question 8 here
Write the SQL query that performs grouping by age groups and displays the maximum salary for each group, excluding groups where the maximum salary is not greater than 8000. 

Note: Calculate the age group as multiples of 5.

Eg., 20,22,23 comes in age group 20. 

25,27,29 comes in age group 25.

Sample table: customer1



For example:

Result
age_group   MAX(salary)
----------  -----------
20          10000
25          8500

```sql
-- Paste your SQL code below for Question 8
SELECT 
    age / 5 * 5 AS age_group,
    MAX(salary) AS "MAX(salary)"
FROM customer1
GROUP BY age / 5 * 5
HAVING MAX(salary) > 8000;

```

**Output:**

<img width="909" height="538" alt="image" src="https://github.com/user-attachments/assets/05519bc6-ef09-4724-8b21-9da80259b702" />

**Question 9**
---
-- Paste Question 9 here
Write an SQL query that groups the customer data into 5-year age intervals, calculates the minimum salary for each group, and excludes groups where the minimum salary is not less than 2000.

Table: customer1



For example:

Result
age_group   MIN(salary)
----------  -----------
25          1500

```sql
-- Paste your SQL code below for Question 9
SELECT 
    age / 5 * 5 AS age_group,
    MIN(salary) AS "MIN(salary)"
FROM customer1
GROUP BY age / 5 * 5
HAVING MIN(salary) < 2000;

```

**Output:**

<img width="931" height="468" alt="image" src="https://github.com/user-attachments/assets/e58cd586-4d9e-47d9-8393-a063df575e3e" />

## RESULT
Thus, the SQL queries to implement aggregate functions, GROUP BY, and HAVING clause have been executed successfully.
