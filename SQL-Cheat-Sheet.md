# SQL Cheat Sheet

### **1. Data Manipulation Language (DML)**

| Command | Description | Syntax | Example |
|---------|-------------|--------|---------|
| SELECT  | Retrieve data from a database | `SELECT columns FROM table WHERE condition;` | `SELECT name, age FROM employees WHERE age > 30;` |
| INSERT  | Add new rows to a table | `INSERT INTO table (columns) VALUES (values);` | `INSERT INTO employees (name, age) VALUES ('John', 28);` |
| UPDATE  | Modify existing data in a table | `UPDATE table SET column = value WHERE condition;` | `UPDATE employees SET age = 30 WHERE name = 'John';` |
| DELETE  | Remove data from a table | `DELETE FROM table WHERE condition;` | `DELETE FROM employees WHERE age < 25;` |
| MERGE   | Perform insert, update, or delete operations | `MERGE INTO target USING source ON condition WHEN MATCHED THEN UPDATE ... WHEN NOT MATCHED THEN INSERT ...;` | `MERGE INTO employees AS e USING new_hires AS n ON e.id = n.id WHEN MATCHED THEN UPDATE SET e.name = n.name WHEN NOT MATCHED THEN INSERT (id, name) VALUES (n.id, n.name);` |

### **2. Data Definition Language (DDL)**

| Command | Description | Syntax | Example |
|---------|-------------|--------|---------|
| CREATE  | Create a new table, database, index, or view | `CREATE TABLE table (columns);` | `CREATE TABLE employees (id INT, name VARCHAR(50), age INT);` |
| ALTER   | Modify an existing database object | `ALTER TABLE table ADD/DROP/MODIFY column;` | `ALTER TABLE employees ADD email VARCHAR(100);` |
| DROP    | Remove an existing database object | `DROP TABLE/ DATABASE/ INDEX object;` | `DROP TABLE employees;` |
| TRUNCATE | Remove all rows from a table | `TRUNCATE TABLE table;` | `TRUNCATE TABLE employees;` |
| RENAME  | Rename a database object | `RENAME TABLE old_name TO new_name;` | `RENAME TABLE employees TO staff;` |

### **3. Data Control Language (DCL)**

| Command | Description | Syntax | Example |
|---------|-------------|--------|---------|
| GRANT   | Give user access privileges to a database | `GRANT privileges ON object TO user;` | `GRANT SELECT, INSERT ON employees TO john;` |
| REVOKE  | Remove user access privileges | `REVOKE privileges ON object FROM user;` | `REVOKE SELECT, INSERT ON employees FROM john;` |

### **4. Transaction Control Language (TCL)**

| Command | Description | Syntax | Example |
|---------|-------------|--------|---------|
| COMMIT  | Save the changes made by the current transaction | `COMMIT;` | `COMMIT;` |
| ROLLBACK | Undo changes made by the current transaction | `ROLLBACK;` | `ROLLBACK;` |
| SAVEPOINT | Set a savepoint within a transaction | `SAVEPOINT savepoint_name;` | `SAVEPOINT sp1;` |
| RELEASE SAVEPOINT | Remove a savepoint | `RELEASE SAVEPOINT savepoint_name;` | `RELEASE SAVEPOINT sp1;` |

### **5. Query Optimization**

| Command | Description | Syntax | Example |
|---------|-------------|--------|---------|
| CREATE INDEX | Create an index to speed up data retrieval | `CREATE INDEX index_name ON table(column);` | `CREATE INDEX idx_age ON employees(age);` |
| EXPLAIN | Show the execution plan of a query | `EXPLAIN query;` | `EXPLAIN SELECT * FROM employees WHERE age > 30;` |
| ANALYZE | Collect statistics about data for the optimizer | `ANALYZE TABLE table;` | `ANALYZE TABLE employees;` |

### **6. Advanced SQL Concepts**

| Concept | Description | Syntax | Example |
|---------|-------------|--------|---------|
| JOIN    | Combine rows from two or more tables | `SELECT columns FROM table1 JOIN table2 ON condition;` | `SELECT * FROM employees e INNER JOIN departments d ON e.dept_id = d.id;` |
| Subquery | Queries within queries | `SELECT column FROM table WHERE column = (subquery);` | `SELECT name FROM employees WHERE dept_id = (SELECT id FROM departments WHERE name = 'HR');` |
| View    | Virtual tables representing the result of a query | `CREATE VIEW view_name AS SELECT columns FROM table WHERE condition;` | `CREATE VIEW emp_view AS SELECT name, age FROM employees WHERE age > 30;` |

### **7. Window Functions**

| Function | Description | Syntax | Example |
|----------|-------------|--------|---------|
| `ROW_NUMBER()` | Assign a unique sequential integer to rows within a partition | `ROW_NUMBER() OVER (PARTITION BY column ORDER BY column)` | `SELECT name, ROW_NUMBER() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rank FROM employees;` |
| `RANK()` | Assign a rank to each row within a partition, with gaps in rank values for ties | `RANK() OVER (PARTITION BY column ORDER BY column)` | `SELECT name, RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rank FROM employees;` |
| `DENSE_RANK()` | Assign a rank to each row within a partition, with no gaps in rank values for ties | `DENSE_RANK() OVER (PARTITION BY column ORDER BY column)` | `SELECT name, DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rank FROM employees;` |
| `NTILE(n)` | Distribute rows into `n` number of approximately equal parts | `NTILE(n) OVER (PARTITION BY column ORDER BY column)` | `SELECT name, NTILE(4) OVER (ORDER BY salary DESC) AS quartile FROM employees;` |
| `SUM()` | Calculate the cumulative sum of a column | `SUM(column) OVER (PARTITION BY column ORDER BY column)` | `SELECT name, salary, SUM(salary) OVER (PARTITION BY dept_id ORDER BY salary DESC) AS cumulative_salary FROM employees;` |
| `AVG()` | Calculate the cumulative average of a column | `AVG(column) OVER (PARTITION BY column ORDER BY column)` | `SELECT name, salary, AVG(salary) OVER (PARTITION BY dept_id ORDER BY salary DESC) AS average_salary FROM employees;` |
| `LEAD()` | Access the value of a column in a subsequent row within the same result set | `LEAD(column, offset, default) OVER (PARTITION BY column ORDER BY column)` | `SELECT name, salary, LEAD(salary, 1, 0) OVER (ORDER BY salary DESC) AS next_salary FROM employees;` |
| `LAG()` | Access the value of a column in a preceding row within the same result set | `LAG(column, offset, default) OVER (PARTITION BY column ORDER BY column)` | `SELECT name, salary, LAG(salary, 1, 0) OVER (ORDER BY salary DESC) AS previous_salary FROM employees;` |

### **8. SQL Functions**

| Function | Description | Syntax | Example |
|----------|-------------|--------|---------|
| `COALESCE()` | Return the first non-null value in a list | `COALESCE(value1, value2, ..., valueN)` | `SELECT name, COALESCE(phone, 'Not Available') AS contact FROM employees;` |
| `NULLIF()` | Return NULL if two expressions are equal | `NULLIF(expression1, expression2)` | `SELECT name, NULLIF(salary, 0) AS salary FROM employees;` |
| `CASE` | Conditional logic within a query | `CASE WHEN condition THEN result ELSE alternative END` | `SELECT name, CASE WHEN salary > 50000 THEN 'High' ELSE 'Low' END AS salary_category FROM employees;` |
| `CAST()` | Convert a value from one data type to another | `CAST(expression AS data_type)` | `SELECT name, CAST(salary AS VARCHAR(10)) AS salary_str FROM employees;` |
| `CONVERT()` | Convert a value from one data type to another (SQL Server specific) | `CONVERT(data_type, expression, style)` | `SELECT name, CONVERT(VARCHAR(10), salary) AS salary_str FROM employees;` |
