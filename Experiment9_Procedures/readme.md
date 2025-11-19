# Experiment 9: PL/SQL – Procedures and Functions

## AIM
To understand and implement procedures and functions in PL/SQL for performing various operations such as calculations, decision-making, and looping.

---

## THEORY

PL/SQL (Procedural Language/SQL) extends SQL by adding procedural constructs like variables, conditions, loops, procedures, and functions. Procedures and functions are subprograms that help modularize the code and improve reusability.

### **Procedure**
A PL/SQL **procedure** is a subprogram that performs a specific action. It does not return a value directly but can return values using `OUT` parameters.

**Syntax:**
```sql
CREATE OR REPLACE PROCEDURE procedure_name (parameters)
IS
BEGIN
   -- statements
END;
```

To call the procedure

```sql
EXEC procedure_name(arguments);
```

### **Function**
A PL/SQL **function** is a subprogram that returns a single value using the RETURN keyword.

```sql
CREATE OR REPLACE FUNCTION function_name (parameters)
RETURN datatype
IS
BEGIN
   -- statements
   RETURN value;
END;
```

To call the function:

```sql
SELECT function_name(arguments) FROM DUAL;
```

Key Differences:

-A procedure does not return a value, whereas a function must return a value.
-Functions can be called from SQL queries, procedures cannot (in most cases).

## 1. Write a PL/SQL Procedure to Find the Square of a Number

### Steps:
- Create a procedure named `find_square`.
- Declare a parameter to accept a number.
- Inside the procedure, compute the square of the input number.
- Use `DBMS_OUTPUT.PUT_LINE` to display the result.
- Call the procedure with a number as input.

```sql
SET SERVEROUTPUT ON;

-- Create procedure to find square of a number
CREATE OR REPLACE PROCEDURE find_square(p_number IN NUMBER) 
IS
    v_result NUMBER;
BEGIN
    v_result := p_number * p_number;
    DBMS_OUTPUT.PUT_LINE('Square of ' || p_number || ' is ' || v_result);
END find_square;
/

-- Calling the procedure
BEGIN
    find_square(6);
    find_square(12);
    find_square(7);
END;
/
```

**Expected Output:**  
Square of 6 is 36

<img width="413" height="170" alt="image" src="https://github.com/user-attachments/assets/bea926b2-1cbd-4028-95b6-b355a51d7d24" />

---

## 2. Write a PL/SQL Function to Return the Factorial of a Number

### Steps:
- Create a function named `get_factorial`.
- Declare a parameter to accept a number.
- Use a loop to calculate the factorial.
- Return the result using the `RETURN` statement.
- Call the function using a `SELECT` statement or in an anonymous block.

```sql
SET SERVEROUTPUT ON;

-- Create function to calculate factorial
CREATE OR REPLACE FUNCTION get_factorial(p_num IN NUMBER) 
RETURN NUMBER
IS
    v_factorial NUMBER := 1;
    v_counter NUMBER;
BEGIN
    -- Handle edge cases
    IF p_num < 0 THEN
        RETURN NULL; -- Factorial not defined for negative numbers
    ELSIF p_num = 0 OR p_num = 1 THEN
        RETURN 1;
    ELSE
        v_counter := p_num;
        WHILE v_counter > 1 LOOP
            v_factorial := v_factorial * v_counter;
            v_counter := v_counter - 1;
        END LOOP;
        RETURN v_factorial;
    END IF;
END get_factorial;
/

-- Calling the function using SELECT
SELECT get_factorial(5) AS factorial_of_5 FROM DUAL;

-- Calling the function in an anonymous block
BEGIN
    DBMS_OUTPUT.PUT_LINE('Factorial of 5 is ' || get_factorial(5));
    DBMS_OUTPUT.PUT_LINE('Factorial of 0 is ' || get_factorial(0));
    DBMS_OUTPUT.PUT_LINE('Factorial of 7 is ' || get_factorial(7));
END;
/
```

**Expected Output:**  
Factorial of 5 is 120

<img width="409" height="157" alt="image" src="https://github.com/user-attachments/assets/d704e832-4fc3-47e2-b7ab-c994f65699d2" />

---

## 3. Write a PL/SQL Procedure to Check Whether a Number is Even or Odd

### Steps:
- Create a procedure named `check_even_odd`.
- Accept an input parameter.
- Use the `MOD` function to check if the number is divisible by 2.
- Display whether it is Even or Odd using `DBMS_OUTPUT.PUT_LINE`.

```sql
SET SERVEROUTPUT ON;

-- Create procedure to check even or odd
CREATE OR REPLACE PROCEDURE check_even_odd(p_number IN NUMBER) 
IS
BEGIN
    IF MOD(p_number, 2) = 0 THEN
        DBMS_OUTPUT.PUT_LINE(p_number || ' is Even');
    ELSE
        DBMS_OUTPUT.PUT_LINE(p_number || ' is Odd');
    END IF;
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: Invalid input');
END check_even_odd;
/

-- Calling the procedure
BEGIN
    check_even_odd(12);
    check_even_odd(15);
    check_even_odd(0);
    check_even_odd(-7);
    check_even_odd(23);
END;
/
```


**Expected Output:**  
12 is Even

<img width="413" height="209" alt="image" src="https://github.com/user-attachments/assets/6985d266-aa2a-45bc-ab02-4aecd1690ba8" />

---

## 4. Write a PL/SQL Function to Return the Reverse of a Number

### Steps:
- Create a function named `reverse_number`.
- Accept an input number as parameter.
- Use a loop to reverse the digits of the number.
- Return the reversed number.
- Call the function and display the output.

```sql
SET SERVEROUTPUT ON;

-- First, drop the function if it exists to avoid conflicts
BEGIN
    EXECUTE IMMEDIATE 'DROP FUNCTION reverse_number';
EXCEPTION
    WHEN OTHERS THEN
        NULL; -- Function doesn't exist, continue
END;
/

-- Create function to reverse a number
CREATE OR REPLACE FUNCTION reverse_number(p_num IN NUMBER) 
RETURN NUMBER
IS
    v_original NUMBER := p_num;
    v_reversed NUMBER := 0;
    v_digit NUMBER;
BEGIN
    -- Handle NULL input
    IF p_num IS NULL THEN
        RETURN NULL;
    END IF;
    
    -- Handle negative numbers
    IF p_num < 0 THEN
        RETURN -reverse_number(ABS(p_num));
    END IF;
    
    -- Handle single digit numbers
    IF p_num < 10 THEN
        RETURN p_num;
    END IF;
    
    -- Reverse the digits
    WHILE v_original > 0 LOOP
        v_digit := MOD(v_original, 10);
        v_reversed := (v_reversed * 10) + v_digit;
        v_original := TRUNC(v_original / 10);
    END LOOP;
    
    RETURN v_reversed;
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error in reverse_number: ' || SQLERRM);
        RETURN NULL;
END reverse_number;
/

-- Verify the function compiled successfully
SELECT object_name, object_type, status 
FROM user_objects 
WHERE object_name = 'REVERSE_NUMBER';

-- Test 1: Calling the function in an anonymous block
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== Testing Reverse Number Function ===');
    DBMS_OUTPUT.PUT_LINE('Reversed number of 1234 is ' || reverse_number(1234));
    DBMS_OUTPUT.PUT_LINE('Reversed number of 56789 is ' || reverse_number(56789));
    DBMS_OUTPUT.PUT_LINE('Reversed number of 100 is ' || reverse_number(100));
    DBMS_OUTPUT.PUT_LINE('Reversed number of -456 is ' || reverse_number(-456));
    DBMS_OUTPUT.PUT_LINE('Reversed number of 7 is ' || reverse_number(7));
    DBMS_OUTPUT.PUT_LINE('Reversed number of 0 is ' || reverse_number(0));
END;
/

-- Test 2: Using SELECT statement (make sure you're in SQL*Plus or SQL Developer)
SELECT reverse_number(1234) AS reversed_number FROM DUAL;
```

**Expected Output:**  
Reversed number of 1234 is 4321

<img width="186" height="89" alt="image" src="https://github.com/user-attachments/assets/30b5296c-9087-44db-87b7-5dfdf30a1a76" />

---

## 5. Write a PL/SQL Procedure to Display the Multiplication Table of a Number

### Steps:
- Create a procedure named `print_table`.
- Accept an input number.
- Use a loop from 1 to 10 to multiply the input number.
- Display the multiplication results using `DBMS_OUTPUT.PUT_LINE`.

```sql
-- Create the procedure
CREATE OR REPLACE PROCEDURE print_table(p_num IN NUMBER) 
IS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Multiplication table of ' || p_num || ':');
    
    -- Loop from 1 to 10 and display multiplication results
    FOR i IN 1..10 LOOP
        DBMS_OUTPUT.PUT_LINE(p_num || ' x ' || i || ' = ' || (p_num * i));
    END LOOP;
END print_table;
/

-- Call the procedure
BEGIN
    print_table(5);
END;
/
```

**Expected Output:**  
Multiplication table of 5:  
5 x 1 = 5  
5 x 2 = 10  
5 x 3 = 15  
...  
5 x 10 = 50

<img width="410" height="321" alt="image" src="https://github.com/user-attachments/assets/40a8be7e-3858-4aab-b8bb-0da285c0b5b3" />


## RESULT
Thus, the PL/SQL programs using procedures and functions were written, compiled, and executed successfully.
