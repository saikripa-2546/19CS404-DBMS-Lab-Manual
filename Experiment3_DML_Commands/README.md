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
Create a table named Products with the following constraints:

ProductID should be the primary key. ProductName should be NOT NULL. Price is of real datatype and should be greater than 0. Stock is of integer datatype and should be greater than or equal to 0. For example:

Test Result INSERT INTO Products VALUES (1, NULL,0,5); Error: NOT NULL constraint failed: Products.ProductName

```
CREATE TABLE Products(
ProductID INTEGER PRIMARY KEY,
ProductName NOT NULL,
Price REAL CHECK (Price>0),
Stock INTEGER CHECK (Stock>=0));
```

**Output:**

<img width="1227" height="353" alt="image" src="https://github.com/user-attachments/assets/41b0a864-0250-46af-b8f1-eef8d97c45d7" />

**Question 2**
Create a table named Customers with the following columns:

CustomerID as INTEGER Name as TEXT Email as TEXT JoinDate as DATETIME For example:

Test Result pragma table_info('Customers'); cid name type notnull dflt_value pk

0 CustomerID INTEGER 0 0 1 Name TEXT 0 0 2 Email TEXT 0 0 3 JoinDate DATETIME 0 0
```
CREATE TABLE Customers(
CustomerID INTEGER,
Name TEXT,
Email TEXT,
JoinDate DATETIME);
```

**Output:**
<img width="1200" height="458" alt="image" src="https://github.com/user-attachments/assets/a457e6b0-a520-4d6e-a186-53934f9cc21e" />

**Question 3**
Create a table named Invoices with the following constraints: InvoiceID as INTEGER should be the primary key. InvoiceDate as DATE. Amount as REAL should be greater than 0. DueDate as DATE should be greater than the InvoiceDate. OrderID as INTEGER should be a foreign key referencing Orders(OrderID). For example:

Test Result INSERT INTO Orders (OrderID, OrderDate, CustomerID) VALUES (1, '2024-08-01', 1); INSERT INTO Invoices (InvoiceID, InvoiceDate, Amount, DueDate, OrderID) VALUES (1, '2024-08-01', 100.0, '2024-09-01', 1); SELECT * FROM Invoices; InvoiceID InvoiceDate Amount DueDate OrderID

1 2024-08-01 100.0 2024-09-01 1
```
CREATE TABLE Invoices(
InvoiceID INTEGER PRIMARY KEY,
InvoiceDate DATE,
Amount REAL CHECK (Amount>0),
DueDate DATE CHECK (DueDate>InvoiceDate),
OrderID INTEGER,
FOREIGN KEY (OrderID) REFERENCES Orders(OrderID));
```
**Output:**
<img width="1221" height="358" alt="image" src="https://github.com/user-attachments/assets/e8d61d4b-853c-4197-b45a-bcac073de069" />

**Question 4**
Insert a book with ISBN 978-1234567890, Title Data Science Essentials, Author Jane Doe, Publisher TechBooks, and Year 2024 into the Books table.

For example:

Test Result SELECT * FROM Books; ISBN Title Author Publisher Year

978-1234567890 Data Science Essentials Jane Doe TechBooks 2024
```
INSERT INTO Books(ISBN, Title, Author, Publisher,Year) VALUES ('978-1234567890','Data Science Essentials','Jane Doe','TechBooks',2024);
```

**Output:**

<img width="1218" height="410" alt="image" src="https://github.com/user-attachments/assets/55c3905a-4622-4593-bcd6-23e324273371" />

**Question 5**
Insert the following employees into the Employee table:

EmployeeID Name Position Department Salary

2 John Smith Developer IT 75000 3 Anna Bell Designer Marketing 68000 For example:

Test Result SELECT * FROM Employee;

EmployeeID Name Position Department Salary

2 John Smith Developer IT 75000 3 Anna Bell Designer Marketing 68000
```
INSERT INTO Employee(EmployeeID,Name,Position,Department,Salary)VALUES(2,'John Smith','Developer','IT',75000);
INSERT INTO Employee(EmployeeID,Name,Position,Department,Salary)VALUES(3,'Anna Bell','Designer','Marketing',68000);
```

**Output:**

<img width="1215" height="420" alt="image" src="https://github.com/user-attachments/assets/87f116dd-05b1-4126-8d2a-fe8d9e36b202" />

**Question 6**
Insert all customers from Old_customers into Customers

Table attributes are CustomerID, Name, Address, Email

For example:

Test Result select * from Customers; CustomerID Name Address Email

301 Michael Johnson 123 Elm Street michael.j@example.com 302 Sarah Lee 456 Oak Avenue sarah.lee@example.com 303 David Wilson 789 Pine Road david.w@example.com
```
INSERT INTO Customers
(CustomerID,Name,Address, Email)
SELECT CustomerID,  Name, Address, Email FROM Old_customers;
```

**Output:**

<img width="1226" height="369" alt="image" src="https://github.com/user-attachments/assets/3b208a6b-7c56-424b-8dc7-1051754a0d8f" />

**Question 7**
Create a table named Events with the following columns:

EventID as INTEGER EventName as TEXT EventDate as DATE For example:

Test Result pragma table_info('Events'); cid name type notnull dflt_value pk

0 EventID INTEGER 0 0 1 EventName TEXT 0 0 2 EventDate DATE 0 0
```
create table Events (
EventID INTEGER,
EventName TEXT,
EventDate DATE
);
```

**Output:**

<img width="1197" height="436" alt="image" src="https://github.com/user-attachments/assets/f24b2cbb-68e8-4c24-9c03-5eb8481975f7" />

**Question 8**
Create a table named Invoices with the following constraints:

InvoiceID as INTEGER should be the primary key. InvoiceDate as DATE. DueDate as DATE should be greater than the InvoiceDate. Amount as REAL should be greater than 0. For example:

Test Result INSERT INTO Invoices (InvoiceID, InvoiceDate) VALUES (1, '2024-08-08'),(1,'2024-09-08'); Error: UNIQUE constraint failed: Invoices.InvoiceID
```
CREATE TABLE Invoices(
InvoiceID INTEGER PRIMARY KEY,
InvoiceDate DATE,
DueDate DATE CHECK (DueDate>InvoiceDate),
Amount REAL CHECK (Amount>0));
```
**Output:**

<img width="1223" height="348" alt="image" src="https://github.com/user-attachments/assets/99ad6f10-7052-4dfa-8fdd-decc9f590de4" />

**Question 9**
Create a table named Shipments with the following constraints: ShipmentID as INTEGER should be the primary key. ShipmentDate as DATE. SupplierID as INTEGER should be a foreign key referencing Suppliers(SupplierID). OrderID as INTEGER should be a foreign key referencing Orders(OrderID). For example:

Test Result INSERT INTO Shipments (ShipmentID, ShipmentDate, SupplierID, OrderID) VALUES (2, '2024-08-03', 99, 1); Error: FOREIGN KEY constraint failed
```
CREATE TABLE Shipments(
ShipmentID INTEGER PRIMARY KEY,
ShipmentDate DATE,
SupplierID INTEGER,

OrderID INTEGER,
FOREIGN KEY (SupplierID) REFERENCES Suppliers(SupplierID),
FOREIGN KEY (OrderID) REFERENCES Orders(OrderID));
```

**Output:**

<img width="1226" height="310" alt="image" src="https://github.com/user-attachments/assets/636b02fd-8940-4f55-9ab7-8d1f8bd6ed40" />


**Question 10**
Write an SQL command can to add a column named email of type TEXT to the customers table

For example:

Test Result pragma table_info('Customers'); cid name type notnull dflt_value pk

0 id integer 0 0 1 name text 0 0 2 email TEXT 0 0
```
ALTER TABLE customers ADD COLUMN email TEXT;
```

**Output:**

<img width="1217" height="357" alt="image" src="https://github.com/user-attachments/assets/55be7bb9-5d5f-4990-bf7e-0356d10c4158" />

## RESULT
Thus, the SQL queries to implement DML commands have been executed successfully.
