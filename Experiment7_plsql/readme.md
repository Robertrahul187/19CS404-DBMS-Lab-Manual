# Experiment 7: PL/SQL – Variables, Control Structures and Loops

## AIM
To write and execute simple PL/SQL programs using variables, loops, and conditional statements.


## THEORY

PL/SQL, which stands for Procedural Language extensions to the Structured Query Language (SQL). It is a combination of SQL along with the procedural features of programming languages.

**Syntax:**
```sql
DECLARE 
   <declarations section> 
BEGIN 
   <executable command(s)>
EXCEPTION 
   <exception handling> 
END;
```

### Basic Components of PL/SQL Block:
- DECLARE: Section to declare variables and constants.
- BEGIN: The execution section that contains PL/SQL statements.
- EXCEPTION: Handles errors or exceptions that occur in the program.
- END: Marks the end of the PL/SQL block.

# PL/SQL Programs – Steps and Expected Output

## 1. Write a PL/SQL program to find the Greatest of Two Numbers

### Steps:
- Declare two numeric variables and initialize them.
- Use an `IF` statement to compare the values.
- Display the greater number using `DBMS_OUTPUT.PUT_LINE`.

**Expected Output:**  
Greater number is: 80

```sql
-- Paste your SQL code below for Question 1
DECLARE
    num1 NUMBER := 80;
    num2 NUMBER := 45;
    result NUMBER;
BEGIN
    IF num1 > num2 THEN
        result := num1;
    ELSE
        result := num2;
    END IF;
    
    DBMS_OUTPUT.PUT_LINE('Greater number is: ' || result);
END;
/

```

**Output:**

<img width="871" height="366" alt="image" src="https://github.com/user-attachments/assets/aee59616-259f-408d-94d5-aaf4f018cf92" />

---

## 2. Write a PL/SQL program to Calculate Sum of First N Natural Numbers

### Steps:
- Declare a variable `n` and assign a value (e.g., 10).
- Initialize a `sum` variable to 0.
- Use a `WHILE` loop to iterate from 1 to `n`, adding each number to the sum.
- Display the result using `DBMS_OUTPUT.PUT_LINE`.

**Expected Output:**  
Sum of first 10 natural numbers is: 55

```sql
-- Paste your SQL code below for Question 2
DECLARE
    n NUMBER := 10;
    sum_val NUMBER := 0;
    i NUMBER := 1;
BEGIN
    WHILE i <= n LOOP
        sum_val := sum_val + i;
        i := i + 1;
    END LOOP;
    
    DBMS_OUTPUT.PUT_LINE('Sum of first ' || n || ' natural numbers is: ' || sum_val);
END;
/

```

**Output:**

<img width="883" height="367" alt="image" src="https://github.com/user-attachments/assets/cf85bc5b-5163-42b9-87b3-3c374052a59c" />

---

## 3. Write a PL/SQL program to generate Fibonacci series

### Steps:
- Declare the variable `n` to indicate how many terms to generate.
- Initialize the first two Fibonacci numbers (0 and 1).
- Use a loop to generate the next terms using the formula `c = a + b`.
- Print each term in the series.

**Expected Output:**  
n = 7  
Fibonacci sequence: 0, 1, 1, 2, 3, 5, 8

```sql
-- Paste your SQL code below for Question 3
DECLARE
    n NUMBER := 7;
    a NUMBER := 0;
    b NUMBER := 1;
    c NUMBER;
    i NUMBER;
BEGIN
    DBMS_OUTPUT.PUT('Fibonacci sequence: ' || a || ', ' || b);
    
    FOR i IN 3..n LOOP
        c := a + b;
        DBMS_OUTPUT.PUT(', ' || c);
        a := b;
        b := c;
    END LOOP;
    
    DBMS_OUTPUT.NEW_LINE;
END;
/

```

**Output:**

<img width="894" height="375" alt="image" src="https://github.com/user-attachments/assets/3db3fef9-e112-4434-937f-531e931b9616" />

---

## 4. Write a PL/SQL Program to display the number in Reverse Order

### Steps:
- Declare a variable `n` and assign a value (e.g., 1535).
- Use a loop to extract each digit using modulo and reverse the number.
- Display the reversed number.

**Expected Output:**  
n = 1535  
Reversed number is 5351

```sql
-- Paste your SQL code below for Question 4
DECLARE
    n NUMBER := 1535;
    reversed_num NUMBER := 0;
    temp NUMBER;
BEGIN
    temp := n;
    
    WHILE temp > 0 LOOP
        reversed_num := reversed_num * 10 + MOD(temp, 10);
        temp := FLOOR(temp / 10);
    END LOOP;
    
    DBMS_OUTPUT.PUT_LINE('n = ' || n);
    DBMS_OUTPUT.PUT_LINE('Reversed number is ' || reversed_num);
END;
/

```

**Output:**

<img width="882" height="363" alt="image" src="https://github.com/user-attachments/assets/0e798efe-908b-490e-9a12-edade2a393ec" />

---

## 5. Write a PL/SQL program to find the largest of three numbers

### Steps:
- Declare three numeric variables `a`, `b`, and `c`.
- Use nested `IF-ELSIF-ELSE` conditions to find the largest among the three.
- Display the largest number.

**Expected Output:**  
a = 10, b = 9, c = 15  
Largest of three number is 15

```sql
-- Paste your SQL code below for Question 5
DECLARE
    a NUMBER := 10;
    b NUMBER := 9;
    c NUMBER := 15;
    largest NUMBER;
BEGIN
    IF a >= b AND a >= c THEN
        largest := a;
    ELSIF b >= a AND b >= c THEN
        largest := b;
    ELSE
        largest := c;
    END IF;
    
    DBMS_OUTPUT.PUT_LINE('a = ' || a || ', b = ' || b || ', c = ' || c);
    DBMS_OUTPUT.PUT_LINE('Largest of three number is ' || largest);
END;
/

```

**Output:**

<img width="881" height="366" alt="image" src="https://github.com/user-attachments/assets/7af4999b-7201-4fcf-9b84-ec8c26ac20e4" />

## RESULT
Thus, the PL/SQL programs using variables, conditionals, and loops were executed successfully.

