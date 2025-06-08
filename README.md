SQL Interview Questions and Concepts
This document covers essential SQL concepts and commands, tailored for interview preparation. It includes practical examples, syntax, and common interview questions to help you master SQL.
SQL Command Categories
SQL commands are broadly categorized into five types: DDL, DML, DCL, TCL, and DQL. Below, each category is explained with examples and interview-focused insights.
1. Data Definition Language (DDL)

Purpose: Modifies the structure of database objects (e.g., tables).

Key Feature: Auto-committed, meaning changes are permanently saved.

Commands:

CREATE: Creates a new table or database object.CREATE TABLE EMPLOYEES (
    ID INT NOT NULL,
    NAME VARCHAR(20) NOT NULL,
    AGE INT,
    SALARY DECIMAL(10, 2),
    PRIMARY KEY (ID)
);


ALTER: Modifies an existing table structure.ALTER TABLE EMPLOYEES ADD EMAIL VARCHAR(100);
ALTER TABLE EMPLOYEES MODIFY NAME VARCHAR(30);


DROP: Deletes a table and all its data.DROP TABLE EMPLOYEES;


TRUNCATE: Removes all rows from a table, freeing space.TRUNCATE TABLE EMPLOYEES;




Interview Question: What is the difference between DROP and TRUNCATE?

Answer: DROP removes the table structure and data permanently, requiring recreation to use the table again. TRUNCATE deletes only the data, retaining the table structure, and is faster for large tables.



2. Data Manipulation Language (DML)

Purpose: Modifies data within a table.

Key Feature: Not auto-committed; changes can be rolled back.

Commands:

INSERT: Adds new rows to a table.INSERT INTO EMPLOYEES (ID, NAME, AGE, SALARY) VALUES (1, 'John Doe', 30, 50000.00);


UPDATE: Modifies existing data.UPDATE EMPLOYEES SET SALARY = 55000.00 WHERE ID = 1;


DELETE: Removes rows from a table.DELETE FROM EMPLOYEES WHERE AGE < 25;




Interview Question: How would you update multiple columns in a table with a condition?

Answer: Use the UPDATE statement with SET for multiple columns and a WHERE clause for conditions.UPDATE EMPLOYEES SET SALARY = 60000.00, AGE = 31 WHERE ID = 1;





3. Data Control Language (DCL)

Purpose: Manages access and permissions.

Commands:

GRANT: Assigns access privileges.GRANT SELECT, INSERT ON EMPLOYEES TO USER1;


REVOKE: Removes access privileges.REVOKE SELECT ON EMPLOYEES FROM USER1;




Interview Question: What is the difference between GRANT and REVOKE?

Answer: GRANT provides permissions to a user, while REVOKE removes previously granted permissions.



4. Transaction Control Language (TCL)

Purpose: Manages transactions to ensure data integrity.

Commands:

COMMIT: Saves all changes made in a transaction.DELETE FROM EMPLOYEES WHERE AGE = 30;
COMMIT;


ROLLBACK: Undoes uncommitted changes.DELETE FROM EMPLOYEES WHERE AGE = 30;
ROLLBACK;


SAVEPOINT: Sets a point to roll back to without undoing the entire transaction.SAVEPOINT before_update;
UPDATE EMPLOYEES SET SALARY = 60000.00 WHERE ID = 1;
ROLLBACK TO before_update;




Interview Question: How does SAVEPOINT work in a transaction?

Answer: SAVEPOINT marks a point in a transaction to which you can roll back without undoing all changes. It’s useful for partial rollbacks in complex transactions.



5. Data Query Language (DQL)

Purpose: Retrieves data from the database.

Command:

SELECT: Fetches specific data based on conditions.SELECT NAME, SALARY FROM EMPLOYEES WHERE AGE > 25;




Interview Question: What is the difference between SELECT and SELECT DISTINCT?

Answer: SELECT retrieves all rows, including duplicates, while SELECT DISTINCT eliminates duplicate rows, returning only unique records.



Key SQL Concepts for Interviews
Constraints
Constraints enforce rules on table data to ensure accuracy and reliability.

NOT NULL: Ensures a column cannot have NULL values.
CREATE TABLE EMPLOYEES (
    ID INT NOT NULL,
    NAME VARCHAR(20) NOT NULL
);


DEFAULT: Sets a default value for a column if none is provided.
ALTER TABLE EMPLOYEES MODIFY SALARY DECIMAL(10, 2) DEFAULT 5000.00;


UNIQUE: Ensures all values in a column are distinct.
ALTER TABLE EMPLOYEES ADD CONSTRAINT unique_email UNIQUE (EMAIL);


PRIMARY KEY: Uniquely identifies each row; cannot be NULL.
ALTER TABLE EMPLOYEES ADD PRIMARY KEY (ID);


FOREIGN KEY: Links two tables by referencing a primary key.
ALTER TABLE ORDERS ADD FOREIGN KEY (CUSTOMER_ID) REFERENCES EMPLOYEES (ID);


CHECK: Ensures values meet a specific condition.
ALTER TABLE EMPLOYEES ADD CONSTRAINT check_age CHECK (AGE >= 18);


Interview Question: What happens if you try to insert a duplicate value into a column with a UNIQUE constraint?

Answer: The database will reject the insert operation and throw an error, ensuring data uniqueness.



Joins
Joins combine data from multiple tables based on a related column.

INNER JOIN: Returns rows with matching values in both tables.
SELECT C.NAME, O.AMOUNT
FROM CUSTOMERS C
INNER JOIN ORDERS O ON C.ID = O.CUSTOMER_ID;


LEFT JOIN: Returns all rows from the left table, with NULLs for non-matching rows from the right table.
SELECT C.NAME, O.AMOUNT
FROM CUSTOMERS C
LEFT JOIN ORDERS O ON C.ID = O.CUSTOMER_ID;


RIGHT JOIN: Returns all rows from the right table, with NULLs for non-matching rows from the left table.
SELECT C.NAME, O.AMOUNT
FROM CUSTOMERS C
RIGHT JOIN ORDERS O ON C.ID = O.CUSTOMER_ID;


FULL JOIN: Returns all rows from both tables, with NULLs for non-matching rows.
SELECT C.NAME, O.AMOUNT
FROM CUSTOMERS C
FULL JOIN ORDERS O ON C.ID = O.CUSTOMER_ID;


SELF JOIN: Joins a table to itself.
SELECT A.NAME, B.NAME
FROM EMPLOYEES A, EMPLOYEES B
WHERE A.MANAGER_ID = B.ID;


CARTESIAN JOIN: Returns the Cartesian product of two tables.
SELECT C.NAME, O.AMOUNT
FROM CUSTOMERS C, ORDERS O;


Interview Question: What is the difference between INNER JOIN and LEFT JOIN?

Answer: INNER JOIN returns only matching rows from both tables, while LEFT JOIN returns all rows from the left table and matching rows from the right table, with NULLs for non-matches.



Aggregate Functions
Aggregate functions perform calculations on a set of values.

COUNT: Returns the number of rows.
SELECT COUNT(*) FROM EMPLOYEES WHERE SALARY > 5000;


SUM: Calculates the total of a numeric column.
SELECT SUM(SALARY) FROM EMPLOYEES;


AVG: Calculates the average of a numeric column.
SELECT AVG(SALARY) FROM EMPLOYEES;


MIN/MAX: Returns the smallest/largest value.
SELECT MIN(SALARY), MAX(SALARY) FROM EMPLOYEES;


Interview Question: How would you find the second highest salary in a table?

Answer:SELECT MAX(SALARY)
FROM EMPLOYEES
WHERE SALARY NOT IN (SELECT MAX(SALARY) FROM EMPLOYEES);





Advanced Queries

IN Operator: Filters rows based on a list of values.
SELECT * FROM EMPLOYEES WHERE DEPARTMENT IN ('HR', 'IT');


BETWEEN Operator: Selects values within a range.
SELECT * FROM EMPLOYEES WHERE SALARY BETWEEN 5000 AND 10000;


LIKE Operator: Matches patterns using wildcards (% and _).
SELECT * FROM EMPLOYEES WHERE NAME LIKE 'J%';


GROUP BY: Groups rows with identical values for aggregation.
SELECT DEPARTMENT, AVG(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT;


HAVING: Filters grouped results.
SELECT DEPARTMENT, COUNT(*)
FROM EMPLOYEES
GROUP BY DEPARTMENT
HAVING COUNT(*) > 5;


UNION/UNION ALL: Combines results from multiple SELECT statements.
SELECT NAME FROM EMPLOYEES
UNION
SELECT NAME FROM CONTRACTORS;


Interview Question: What is the difference between UNION and UNION ALL?

Answer: UNION removes duplicate rows from the result set, while UNION ALL includes all rows, including duplicates, making it faster.



Views

Purpose: Virtual tables based on a query’s result set.

Syntax:
CREATE VIEW HIGH_EARNERS AS
SELECT NAME, SALARY
FROM EMPLOYEES
WHERE SALARY > 10000;


WITH CHECK OPTION: Ensures updates/inserts meet view conditions.
CREATE VIEW YOUNG_EMPLOYEES AS
SELECT NAME, AGE
FROM EMPLOYEES
WHERE AGE < 30
WITH CHECK OPTION;


Interview Question: Can you update a view?

Answer: Yes, a view can be updated if it meets specific conditions (e.g., no DISTINCT, no aggregate functions, single table). Updates affect the underlying table.



Common Interview Questions

How do you optimize a slow SQL query?

Analyze the query execution plan.
Use appropriate indexes on frequently queried columns.
Avoid SELECT *; specify only needed columns.
Use joins instead of subqueries where possible.
Filter early with WHERE clauses.


What is a self-join, and when would you use it?

A self-join joins a table to itself, useful for hierarchical data (e.g., employees and their managers).
Example:SELECT e1.NAME, e2.NAME AS MANAGER
FROM EMPLOYEES e1
INNER JOIN EMPLOYEES e2 ON e1.MANAGER_ID = e2.ID;




What is the difference between a primary key and a foreign key?

Primary Key: Uniquely identifies each row in a table, cannot be NULL.
Foreign Key: References a primary key in another table to establish a relationship.


How would you find duplicate records in a table?

Use GROUP BY and HAVING to identify duplicates.SELECT NAME, COUNT(*)
FROM EMPLOYEES
GROUP BY NAME
HAVING COUNT(*) > 1;




What is the purpose of the HAVING clause?

Filters grouped results from a GROUP BY, used with aggregate functions like COUNT, SUM, etc.
Example:SELECT DEPARTMENT, AVG(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT
HAVING AVG(SALARY) > 5000;





Practical Example: Database Schema
Consider two tables:

TEACHERS: TID (PK), TNAME, TCITY
TEACHER_DETAILS: TDATE, T2ID2 (PK), T2NAME, TID (FK)

Query to join tables:
SELECT T.TNAME, TD.T2NAME, TD.TDATE
FROM TEACHERS T
INNER JOIN TEACHER_DETAILS TD ON T.TID = TD.TID;

Interview Question: Write a query to find teachers with details after a specific date.

Answer:SELECT T.TNAME, TD.T2NAME, TD.TDATE
FROM TEACHERS T
INNER JOIN TEACHER_DETAILS TD ON T.TID = TD.TID
WHERE TD.TDATE > '2023-01-01';



Tips for SQL Interviews

Practice Writing Queries: Be comfortable with writing complex queries involving joins, subqueries, and aggregates.
Understand Indexes: Know how indexes improve query performance but may slow down inserts/updates.
Explain Your Thought Process: When solving problems, articulate how you approach the query, including optimization considerations.
Know Database-Specific Syntax: Be aware of differences (e.g., MySQL’s LIMIT vs. SQL Server’s TOP).
Test Your Queries: Use sample datasets to verify your queries return the expected results.

This guide provides a comprehensive overview of SQL concepts and commands, with a focus on interview preparation. Practice these examples and questions to build confidence in your SQL skills.

