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

### PL/SQL Query:

```
CREATE TABLE employees (
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50)
);

CREATE TABLE employee_log (
    log_id NUMBER GENERATED ALWAYS AS IDENTITY,
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50),
    log_date DATE
);

CREATE OR REPLACE TRIGGER trg_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_log (emp_id, emp_name, designation, log_date)
    VALUES (:NEW.emp_id, :NEW.emp_name, :NEW.designation, SYSDATE);
END;
/

INSERT INTO employees VALUES (1, 'John', 'Manager');
INSERT INTO employees VALUES (2, 'Alice', 'Developer');
SELECT * FROM employee_log;
```

### Expected Output:
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.
<br>
<br>
<img width="1000" height="252" alt="image" src="https://github.com/user-attachments/assets/1b62a172-bca4-49ca-84c2-d6bd7b4ec848" />


---

## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.

### PL/SQL Query:

```
CREATE TABLE sensitive_data (
    id NUMBER,
    info VARCHAR2(100)
);

INSERT INTO sensitive_data VALUES (1, 'Confidential Record');

CREATE OR REPLACE TRIGGER trg_prevent_delete
BEFORE DELETE ON sensitive_data
FOR EACH ROW
BEGIN
    RAISE_APPLICATION_ERROR(-20001, 'ERROR: Deletion not allowed on this table.');
END;
/

DELETE FROM sensitive_data WHERE id = 1;
```
### Expected Output:
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`
<br>
<br>
<img width="1178" height="210" alt="image" src="https://github.com/user-attachments/assets/8300189c-db1a-4fb5-b7ea-77dbfcebcab8" />


---

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.

### PL/SQL Query:

```
CREATE TABLE products (
    product_id NUMBER,
    product_name VARCHAR2(50),
    price NUMBER,
    last_modified DATE
);

INSERT INTO products VALUES (1, 'Laptop', 55000, SYSDATE);

CREATE OR REPLACE TRIGGER trg_update_timestamp
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    :NEW.last_modified := SYSDATE;
END;
/

UPDATE products SET price = 60000 WHERE product_id = 1;
SELECT product_id, product_name, price, last_modified FROM products;
```
### Expected Output:
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.
<br>
<br>
<img width="864" height="280" alt="image" src="https://github.com/user-attachments/assets/c3cabd27-4546-4e16-9e24-641705427ea2" />

---

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.

### PL/SQL Query:

```
CREATE TABLE customer_orders (
    order_id NUMBER,
    customer_name VARCHAR2(50),
    amount NUMBER
);

CREATE TABLE audit_log (
    update_count NUMBER
);

INSERT INTO audit_log VALUES (0);
INSERT INTO customer_orders VALUES (1, 'John', 2000);

CREATE OR REPLACE TRIGGER trg_update_count
AFTER UPDATE ON customer_orders
FOR EACH ROW
BEGIN
    UPDATE audit_log SET update_count = update_count + 1;
END;
/

UPDATE customer_orders SET amount = 2500 WHERE order_id = 1;
UPDATE customer_orders SET amount = 2700 WHERE order_id = 1;
SELECT * FROM audit_log;
```
### Expected Output:
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.
<br>
<br>
<img width="715" height="201" alt="image" src="https://github.com/user-attachments/assets/28593a64-4a18-4f7b-900b-c610d577754b" />


---

## 5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.

### PL/SQL Query:

```
CREATE OR REPLACE TRIGGER trg_check_salary
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF :NEW.designation = 'Intern' THEN
        IF :NEW.emp_id < 3000 THEN
            RAISE_APPLICATION_ERROR(-20002, 'ERROR: Salary below minimum threshold.');
        END IF;
    END IF;
END;
/

INSERT INTO employees VALUES (3, 'Bob', 'Intern');
```
### Expected Output:
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`

<br>
<br>
<img width="1074" height="227" alt="image" src="https://github.com/user-attachments/assets/01c20b2c-b24f-4948-a997-935d6008749a" />



## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.





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

### PL/SQL Query:

```
CREATE TABLE employees (
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50)
);

CREATE TABLE employee_log (
    log_id NUMBER GENERATED ALWAYS AS IDENTITY,
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50),
    log_date DATE
);

CREATE OR REPLACE TRIGGER trg_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_log (emp_id, emp_name, designation, log_date)
    VALUES (:NEW.emp_id, :NEW.emp_name, :NEW.designation, SYSDATE);
END;
/

INSERT INTO employees VALUES (1, 'John', 'Manager');
INSERT INTO employees VALUES (2, 'Alice', 'Developer');
SELECT * FROM employee_log;
```

### Expected Output:
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.
<br>
<br>
<img width="1000" height="252" alt="image" src="https://github.com/user-attachments/assets/1b62a172-bca4-49ca-84c2-d6bd7b4ec848" />


---

## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.

### PL/SQL Query:

```
CREATE TABLE sensitive_data (
    id NUMBER,
    info VARCHAR2(100)
);

INSERT INTO sensitive_data VALUES (1, 'Confidential Record');

CREATE OR REPLACE TRIGGER trg_prevent_delete
BEFORE DELETE ON sensitive_data
FOR EACH ROW
BEGIN
    RAISE_APPLICATION_ERROR(-20001, 'ERROR: Deletion not allowed on this table.');
END;
/

DELETE FROM sensitive_data WHERE id = 1;
```
### Expected Output:
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`
<br>
<br>
<img width="1178" height="210" alt="image" src="https://github.com/user-attachments/assets/8300189c-db1a-4fb5-b7ea-77dbfcebcab8" />


---

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.

### PL/SQL Query:

```
CREATE TABLE products (
    product_id NUMBER,
    product_name VARCHAR2(50),
    price NUMBER,
    last_modified DATE
);

INSERT INTO products VALUES (1, 'Laptop', 55000, SYSDATE);

CREATE OR REPLACE TRIGGER trg_update_timestamp
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    :NEW.last_modified := SYSDATE;
END;
/

UPDATE products SET price = 60000 WHERE product_id = 1;
SELECT product_id, product_name, price, last_modified FROM products;
```
### Expected Output:
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.
<br>
<br>
<img width="864" height="280" alt="image" src="https://github.com/user-attachments/assets/c3cabd27-4546-4e16-9e24-641705427ea2" />

---

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.

### PL/SQL Query:

```
CREATE TABLE customer_orders (
    order_id NUMBER,
    customer_name VARCHAR2(50),
    amount NUMBER
);

CREATE TABLE audit_log (
    update_count NUMBER
);

INSERT INTO audit_log VALUES (0);
INSERT INTO customer_orders VALUES (1, 'John', 2000);

CREATE OR REPLACE TRIGGER trg_update_count
AFTER UPDATE ON customer_orders
FOR EACH ROW
BEGIN
    UPDATE audit_log SET update_count = update_count + 1;
END;
/

UPDATE customer_orders SET amount = 2500 WHERE order_id = 1;
UPDATE customer_orders SET amount = 2700 WHERE order_id = 1;
SELECT * FROM audit_log;
```
### Expected Output:
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.
<br>
<br>
<img width="715" height="201" alt="image" src="https://github.com/user-attachments/assets/28593a64-4a18-4f7b-900b-c610d577754b" />


---

## 5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.

### PL/SQL Query:

```
CREATE OR REPLACE TRIGGER trg_check_salary
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF :NEW.designation = 'Intern' THEN
        IF :NEW.emp_id < 3000 THEN
            RAISE_APPLICATION_ERROR(-20002, 'ERROR: Salary below minimum threshold.');
        END IF;
    END IF;
END;
/

INSERT INTO employees VALUES (3, 'Bob', 'Intern');
```
### Expected Output:
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`

<br>
<br>
<img width="1074" height="227" alt="image" src="https://github.com/user-attachments/assets/01c20b2c-b24f-4948-a997-935d6008749a" />



## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.





