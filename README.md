# Experiment 3: DML Commands

## AIM
To study and implement DML (Data Manipulation Language) commands.

## THEORY

### 1. INSERT INTO
Used to add records into a relation.
These are three type of INSERT INTO queries which are as
A)Inserting a single record
**Syntax (Single Row):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES (value_1, value_2, ...);
```
**Syntax (Multiple Rows):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES
(value_1, value_2, ...),
(value_3, value_4, ...);
```
**Syntax (Insert from another table):**
```sql
INSERT INTO table_name SELECT * FROM other_table WHERE condition;
```
### 2. UPDATE
Used to modify records in a relation.
Syntax:
```sql
UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
```
### 3. DELETE
Used to delete records from a relation.
**Syntax (All rows):**
```sql
DELETE FROM table_name;
```
**Syntax (Specific condition):**
```sql
DELETE FROM table_name WHERE condition;
```
### 4. SELECT
Used to retrieve records from a table.
**Syntax:**
```sql
SELECT column1, column2 FROM table_name WHERE condition;
```
**Question 1**
--
Write a SQL statement to Update the product_name to 'Premium Bread' whose product ID is 5 in the products table.
Products table

```sql
update products set product_name ='Premium Bread' where product_id=5;
```

**Output:**

<img width="1246" height="476" alt="image" src="https://github.com/user-attachments/assets/3618808e-bd63-4a59-8ab4-944abc8b8dea" />

**Question 2**
---
Write a SQL statement to Update the grade of all customers in Chennai city as  5. 
Customer table (customer_id,cust_name,city,grade,salesman_id)

```sql
update Customer set grade=5 where city='Chennai';
```

**Output:**

<img width="1235" height="558" alt="image" src="https://github.com/user-attachments/assets/2c6aca59-9809-4855-8c74-025d23275742" />


**Question 3**
---
Salary will be increased by 25% for the department 40, 15% for department 90 and 10% for the department 110 and the rest of the departments will remain same.
Employees table

```sql
update Employees set salary=salary+salary*0.25 where department_id=40;
update Employees set salary=salary+salary*0.15 where department_id=90;
update Employees set salary=salary+salary*0.1 where department_id=110;
```

**Output:**
<img width="1229" height="530" alt="image" src="https://github.com/user-attachments/assets/c735990d-2e94-4081-939e-9ba99fd0b5e8" />


**Question 4**
---
Write a SQL query to Delete all Doctors whose Specialization is either 'Pediatrics' or 'Cardiology' and Last Name is Brown.
Sample table: Doctors
attributes : doctor_id, first_name, last_name, specialization

```sql
delete from Doctors where specialization in('Pediatrics','Cardiology') and last_name='Brown';
```

**Output:**
<img width="1231" height="982" alt="image" src="https://github.com/user-attachments/assets/0f24759f-7183-4284-8ebe-5f11d8789d50" />

**Question 5**
---
Write a SQL query to Delete customers from 'customer' table where 'CUST_CITY' is not 'New York' and 'OUTSTANDING_AMT' is greater than 5000.
Sample table: Customer

```sql
delete from Customer where CUST_CITY!='New York' and OUTSTANDING_AMT>5000;
```

**Output:**

<img width="1216" height="667" alt="image" src="https://github.com/user-attachments/assets/3bc8827a-cc09-4b11-b648-f5ecc099a88b" />


**Question 6**
---
Write a SQL query to Delete customers with 'GRADE' 3 or 'AGENT_CODE' 'A008' whose 'OUTSTANDING_AMT' is less than 5000
Sample table: Customer

```sql
delete from Customer where (GRADE=3 OR AGENT_CODE='A008') and OUTSTANDING_AMT<5000;
```

**Output:**

<img width="1237" height="491" alt="image" src="https://github.com/user-attachments/assets/20488604-6bcd-4e01-a353-27600ecc1547" />


**Question 7**
---
Write a query to fetch last 5 rows in EmployeeInfo table.

```sql
select*from Employeeinfo order by EmpID desc limit 5;
```

**Output:**
<img width="1241" height="402" alt="image" src="https://github.com/user-attachments/assets/c7e1131c-137d-4972-b666-9e945e509613" />

**Question 8**
---
Write a query to get all the records from EmployeePosition table who have joined in the year 2020.

```sql
select *from EmployeePosition where DateOfJoining BETWEEN '2020-01-01' AND '2020-12-31';
```

**Output:**
<img width="1147" height="360" alt="image" src="https://github.com/user-attachments/assets/092a9e40-0dbc-4230-bd3d-ed02ffe6f0ef" />


**Question 9**
---
Write a SQL query to label rows in the Calculations table as 'Even' if value1 is even, otherwise 'Odd'.

```sql
SELECT id,
        value1,
        case
            when value1%2=0 then 'Even'
            ELSE 'Odd'
        end as parity
from Calculations;
```

**Output:**

<img width="955" height="559" alt="image" src="https://github.com/user-attachments/assets/d7e4a59e-de8d-4cff-88a0-1c56046ac14f" />


**Question 10**
---
Write a query to calculate the discounted_price and  discounted_price_percentage for each product from the products table. Return product_id, original_price, discount_percentage, discounted_price, and discounted_price_percentage.
Discounted Price: Calculate the price of the product after applying the discount.
Discounted Price Percentage: Calculate the percentage that the discounted price represents relative to the original price.

```sql
select product_id,original_price,discount_percentage,(original_price-(original_price*discount_percentage)) as discounted_price,
cast(round(((original_price-(original_price*discount_percentage))/original_price)*100,0)as int)||'%' as discounted_price_percentage
from products;
```

**Output:**

<img width="1239" height="405" alt="image" src="https://github.com/user-attachments/assets/fb060bff-6d16-4464-b954-c0a0f62ceb40" />

**COMPLETION STATUS**
![WhatsApp Image 2025-10-17 at 10 37 51 AM](https://github.com/user-attachments/assets/f0fb8d77-8271-43fd-a290-2d35a53a5794)

## RESULT
Thus, the SQL queries to implement DML commands have been executed successfully.
