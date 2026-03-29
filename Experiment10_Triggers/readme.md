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

## Program

```
CREATE OR REPLACE TRIGGER trg_log_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_log (emp_id, emp_name, salary, log_date)
    VALUES (:NEW.emp_id, :NEW.emp_name, :NEW.salary, SYSDATE);
END;
/
INSERT INTO employees VALUES (1, 'Ravi', 'Intern', 3500, 40);
INSERT INTO employees VALUES (2, 'Priya', 'HR', 4500, 50);
COMMIT;
SELECT * FROM employee_log;


```

**Expected Output:**
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.


<img width="832" height="160" alt="image" src="https://github.com/user-attachments/assets/bf6486d7-ce5f-49a2-9e7e-af263b901bf2" />

---

## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.

## Program

```
CREATE TABLE sensitive_data (
    id NUMBER PRIMARY KEY,
    name VARCHAR2(100),
    info VARCHAR2(200)
);
CREATE OR REPLACE TRIGGER prevent_delete_sensitive_data
BEFORE DELETE ON sensitive_data
FOR EACH ROW
BEGIN
    RAISE_APPLICATION_ERROR(-20001, 'Deletion not allowed on SENSITIVE_DATA table!');
END;
/
-- Insert a test row
INSERT INTO sensitive_data (id, name, info) VALUES (1, 'Test', 'Sensitive info');

-- Try deleting (this should fail)
DELETE FROM sensitive_data WHERE id = 1;



```

**Expected Output:**
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`



<img width="785" height="138" alt="image" src="https://github.com/user-attachments/assets/4dcd24f8-1ddf-4298-b8a2-8b10eabc61c0" />

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.

## Program

```
CREATE TABLE products (
    product_id NUMBER PRIMARY KEY,
    product_name VARCHAR2(100),
    price NUMBER,
    last_modified TIMESTAMP
);

CREATE OR REPLACE TRIGGER trg_update_last_modified
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    :NEW.last_modified := SYSTIMESTAMP;
END;
/
INSERT INTO products (product_id, product_name, price) VALUES (1, 'Laptop', 1000);
UPDATE products
SET price = 1200
WHERE product_id = 1;

SELECT * FROM products;

```


**Expected Output:**
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.


<img width="870" height="178" alt="image" src="https://github.com/user-attachments/assets/45f4454f-2fd6-49ce-ac3e-fd23f27fe35c" />

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.

## Program

```

CREATE TABLE audit_log (
    table_name VARCHAR2(50) PRIMARY KEY,
    update_count NUMBER DEFAULT 0
);

INSERT INTO audit_log (table_name, update_count)
VALUES ('CUSTOMER_ORDERS', 0);

CREATE OR REPLACE TRIGGER trg_customer_orders_update
AFTER UPDATE ON customer_orders
FOR EACH ROW
BEGIN
    UPDATE audit_log
    SET update_count = update_count + 1
    WHERE table_name = 'CUSTOMER_ORDERS';
END;
/

UPDATE customer_orders
SET order_status = 'SHIPPED'
WHERE order_id = 101;

SELECT * FROM audit_log;

```


**Expected Output:**
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.

<img width="663" height="190" alt="image" src="https://github.com/user-attachments/assets/3a64ed3d-b40c-4be8-8439-24bffc546351" />


## 5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.

## Program

```
CREATE OR REPLACE TRIGGER trg_employees_salary_check
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF :NEW.SALARY <= 3000 THEN
        RAISE_APPLICATION_ERROR(-20001, 'Salary must be greater than 3000.');
    END IF;
END;
/


INSERT INTO employees (EMP_ID, EMP_NAME, DESIGNATION, SALARY, DEPT_NO)
VALUES (101, 'John Doe', 'Manager', 5000, 10);

INSERT INTO employees (EMP_ID, EMP_NAME, DESIGNATION, SALARY, DEPT_NO)
VALUES (102, 'Jane Smith', 'Analyst', 6000, 20);

```

**Expected Output:**
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`


<img width="555" height="144" alt="image" src="https://github.com/user-attachments/assets/3282af6f-dd17-44c8-b416-28a2c0901d70" />


## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.
**Expected Output:**
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`

## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.
