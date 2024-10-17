# SQL In Depth

**Structured Query Language (SQL)** is used to interact with relational databases, allowing users to manage and query structured data. SQL's syntax follows a standard set of rules, with a focus on performing operations like querying data, updating records, creating tables, and managing databases.

**Key Components of SQL Syntax:**

1. **SELECT Statement**: The `SELECT` statement is used to query data from a database. It retrieves specific columns and rows based on defined criteria.
   *   **Basic Syntax**:

       ```sql
       SELECT column1, column2, ...
       FROM table_name
       WHERE condition;
       ```
   *   **Example**:

       ```sql
       SELECT name, age
       FROM employees
       WHERE department = 'Sales';
       ```
2. **INSERT Statement**: The `INSERT` statement adds new rows to a table.
   *   **Basic Syntax**:

       ```sql
       INSERT INTO table_name (column1, column2, ...)
       VALUES (value1, value2, ...);
       ```
   *   **Example**:

       ```sql
       INSERT INTO employees (name, age, department)
       VALUES ('John Doe', 30, 'Engineering');
       ```
3. **UPDATE Statement**: The `UPDATE` statement modifies existing records in a table.
   *   **Basic Syntax**:

       ```sql
       UPDATE table_name
       SET column1 = value1, column2 = value2, ...
       WHERE condition;
       ```
   *   **Example**:

       ```sql
       UPDATE employees
       SET age = 31
       WHERE name = 'John Doe';
       ```
4. **DELETE Statement**: The `DELETE` statement removes rows from a table based on a condition.
   *   **Basic Syntax**:

       ```sql
       DELETE FROM table_name
       WHERE condition;
       ```
   *   **Example**:

       ```sql
       DELETE FROM employees
       WHERE age > 60;
       ```
5. **CREATE TABLE Statement**: The `CREATE TABLE` statement is used to define a new table and its columns.
   *   **Basic Syntax**:

       ```sql
       CREATE TABLE table_name (
         column1 datatype,
         column2 datatype,
         ...
       );
       ```
   *   **Example**:

       ```sql
       CREATE TABLE employees (
         id INT PRIMARY KEY,
         name VARCHAR(100),
         age INT,
         department VARCHAR(50)
       );
       ```
6. **ALTER TABLE Statement**: The `ALTER TABLE` statement modifies the structure of an existing table.
   *   **Basic Syntax**:

       ```sql
       ALTER TABLE table_name
       ADD column_name datatype;
       ```
   *   **Example** (Add a new column):

       ```sql
       ALTER TABLE employees
       ADD hire_date DATE;
       ```
7. **DROP TABLE Statement**: The `DROP TABLE` statement deletes an entire table, including all its data.
   *   **Basic Syntax**:

       ```sql
       DROP TABLE table_name;
       ```
8. **JOIN Clauses**: `JOIN` operations are used to combine rows from two or more tables based on a related column between them.
   *   **Basic Syntax**:

       ```sql
       SELECT column1, column2, ...
       FROM table1
       INNER JOIN table2
       ON table1.common_column = table2.common_column;
       ```
   *   **Example** (INNER JOIN):

       ```sql
       SELECT employees.name, departments.department_name
       FROM employees
       INNER JOIN departments
       ON employees.department_id = departments.id;
       ```
9. **GROUP BY Clause**: The `GROUP BY` statement groups rows that have the same values in specified columns and allows performing aggregate functions (e.g., COUNT, SUM, AVG).
   *   **Basic Syntax**:

       ```sql
       SELECT column, aggregate_function(column)
       FROM table_name
       WHERE condition
       GROUP BY column;
       ```
   *   **Example**:

       ```sql
       sqlCopy codeSELECT department, COUNT(*)
       FROM employees
       GROUP BY department;
       ```
10. **HAVING Clause**: The `HAVING` clause is used to filter results after `GROUP BY`, applying conditions to aggregated data.

*   **Basic Syntax**:

    ```sql
    SELECT column, aggregate_function(column)
    FROM table_name
    GROUP BY column
    HAVING aggregate_function(column) condition;
    ```
*   **Example**:

    ```sql
    SELECT department, COUNT(*)
    FROM employees
    GROUP BY department
    HAVING COUNT(*) > 10;
    ```

11. **ORDER BY Clause**: The `ORDER BY` clause is used to sort the result set in either ascending (`ASC`) or descending (`DESC`) order.

*   **Basic Syntax**:

    ```sql
    SELECT column1, column2
    FROM table_name
    ORDER BY column1 [ASC|DESC];
    ```
*   **Example**:

    ```sql
    SELECT name, age
    FROM employees
    ORDER BY age DESC;
    ```

12. **LIMIT Clause**: The `LIMIT` clause restricts the number of rows returned by a query.

*   **Basic Syntax**:

    ```sql
    SELECT column1, column2
    FROM table_name
    LIMIT number;
    ```
*   **Example**:

    ```sql
    SELECT name, age
    FROM employees
    LIMIT 5;
    ```

**SQL Functions:**

SQL also supports built-in functions for mathematical, string, date/time, and aggregate operations.

1. **Aggregate Functions**:
   * `COUNT()`: Counts the number of rows.
   * `SUM()`: Returns the total sum of a numeric column.
   * `AVG()`: Calculates the average of a numeric column.
   * `MAX()` / `MIN()`: Returns the maximum or minimum value in a column.
   *   **Example**:

       ```sql
       SELECT AVG(salary)
       FROM employees;
       ```
2. **String Functions**:
   * `UPPER()`, `LOWER()`: Converts strings to uppercase or lowercase.
   * `CONCAT()`: Concatenates two or more strings.
   * `LENGTH()`: Returns the length of a string.
   *   **Example**:

       ```sql
       sqlCopy codeSELECT UPPER(name)
       FROM employees;
       ```
3. **Date/Time Functions**:
   * `NOW()`: Returns the current date and time.
   * `DATEDIFF()`: Returns the difference between two dates.
   * `DATE()`: Extracts the date part of a timestamp.
   *   **Example**:

       ```sql
       SELECT DATEDIFF(NOW(), hire_date)
       FROM employees;
       ```

**Common SQL Best Practices:**

1. **Use Aliases**: Use `AS` to assign short aliases to table and column names for readability.
   *   **Example**:

       ```sql
       SELECT e.name AS employee_name, d.department_name AS department
       FROM employees AS e
       INNER JOIN departments AS d
       ON e.department_id = d.id;
       ```
2. **Avoid SELECT \* in Production**: Always specify the required columns to improve query performance and readability.
   *   **Example**:

       ```sql
       SELECT name, age FROM employees;  -- Better than SELECT * 
       ```
3. **Indexing**: Index columns frequently used in `WHERE` and `JOIN` conditions to speed up query performance.
