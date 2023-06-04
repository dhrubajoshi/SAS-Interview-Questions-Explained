# SQL Interview Questions and Answers Part 1

This is a collection of SQL interview question answers. Most of the questions are commonly asked in the SQL Developer/Programmer/Analyst position interview. Every post has covered 10 related questions and answers.

Basic QA (Part-1):

## 01: What are the mostly used sql Statements?
Mostly commonly used statements are:

1. SELECT:-Retrives data from a table.
2. INSERT:- Adds new data in to a table.
3. DELETE:-Removes existing data from a table.
4. UPDAT:- Modifies the existing data in a table.
5. CREATE OBJECT_TYPE:- Creates a new data_object such as table.
6. ALTER OBJECT_TYPE:- Modifies the structure of an object such as table.
7. DROP OBJECT_TYPE:- Removes existing database object, such as table.

## 02: How many types of SQL Statements are used in SQL?
There are four different types of SQL statement groups:

1. DDL: Data Definition Language (CREATE, DROP, ALTER, TRUNCATE,COMMENT,RENAME)
2. DML: Data Manipulation Language (SELECT, INSERT, UPDATE, DELETE, MERGE)
3. DCL: Data control Language (GRANT, REVOKE)
4. TCL: TRANSACTION CONTROL LANGUAGE(COMMIT, ROLLBACK, SAVEPOINT)

## 03: What is schema in SQL?
A schema is a collection of certain database objects-Tables, Index and View all of which are owned by a user account.

## 04: Difference between View and Data table in sql.
1. A table is used to organize data in the form of rows and columns and displayed them in a structured format. It makes the stored information more understandable to the human. 
Views are treated as virtual/logical table used to view or manipulate parts of the table. It is a database object that contains rows and columns the same as real tables.
2. b.	Table is a physical entity that means data is actually stored in the table.
View is a virtual entity, that means data is not actually stored in the table.
3. Table is used to store the data.
Views is used to extract the data from the table.
4. Table is independent data object.
View depends upon the table. We cannot create views without table.
5. Table occupies the space on the system.
Views do not occupy the spave on the system.


## 05: How to count the number of columns in a table?
Number of column can be count using following code:

```
select count(*) as No_of_Column
from user_tab_columns
where table_name='EMPLOYEES' 

```
Output result: No_of_Column=11

## 06: How do you calculate number of rows in the table.
Number of rows can be calculated using following sql code:

```
select count(*) as Row_Number
from employees;

```
Output result: Row_Number= 107

## 07: What are the window functions in sql?
In SQL, a window function or analytic function is a function which uses values from one or multiple rows to return a value for each row. 

This contrasts with an aggregate function, which returns a single value for multiple rows.

 Window functions have an OVER clause; any function without an OVER clause is not a window function, but rather an aggregate or single-row (scalar) function

- ROW_NUMBER()
- RANK()
- DENSE_RANK()
- NTILE()
- LEAD()
- LAG()
- SUM() OVER (PARTITION BY … ORDER BY …)
- AVG() OVER (PARTITION BY … ORDER BY …)
- MAX() OVER (PARTITION BY … ORDER BY …)
- MIN() OVER (PARTITION BY … ORDER BY …)

## 08: What are the differences between Truncate and Delete statement?

The TRUNCATE and DELETE both statements in SQL are used to remove data from database tables. But they have some differences:

- TRUNCATE is DDL operation but DELETE is DML operation. 
- TRUNCATE removes all data at once keeping table structure. But DELETE statement removes one or more rows based on specified condition. 
- TRUNCATE is fast (Few resource use)but DELETE is slow(Large resource use). 
- TRUNCATE can not be rollback but DELETE can be rollback.

## 09: What do you mean by drop statement and where do you use?

Drop statement in sql is DDL operation which is used to remove database objects such as tables, views, indexes, or other database structures. It deletes entire object and its associated data. All the dependent objects (constraints, triggers) are also dropped and rollback is not possible.

## 10: What are the differences between update and alter statement in sql?
UPDATE is DML operation and used to modify existing data in a table, changing the values within specific rows and columns.

```
Sample query:
UPDATE tablename 
SET column1 = value1 
WHERE condition;
```


ALTER is DDL operation and used to modify the structure of database objects, such as tables, views, or columns, altering their definitions or properties.

```
Sample Query:
ALTER TABLE tablename 
ADD columnname datatype;

```
















  