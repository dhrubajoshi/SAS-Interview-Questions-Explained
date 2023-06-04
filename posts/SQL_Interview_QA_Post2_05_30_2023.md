# SQL Interview Questions and Answers Part 2

This is a collection of SQL interview question answers. Most of the questions are commonly asked in the SQL Developer/Programmer/Analyst position interview. Every post has covered 10 related questions and answers.

Identification and removing row duplication and different keys:

## 01: How do you identify if the given table has duplicate records or not?
First identify the name of columns(Vars) exist in the table and run following sample code.

```
SELECT VAR1
		,VAR2
		,VAR3
		,....
		,COUNT(*)
FROM TABLE_NAME
GROUP BY VAR1
		,VAR2
		,VAR3
		,....
HAVING COUNT(*) > 1;

```
If this code displays one or more than one records then it indicates that table contains duplicate records.


## 02: If the table contains duplicate records then how can you remove duplicate records?


## 03: What do you mean by a query?
, subquery, and correlated sub query.
Query: A query is request for data or information from a database table  or combination of tables.
Sample query:

```
SELECT * FROM EMPLOYEES;

```
## 04: What do you mean by sub query?
Subquery: Query inside a main query is called subquery.It is also known as inner query or nsted query. If a final result depends upon the result of other table then we use subquery. Subquery executes only one time.

Sample code:

```
SELECT * FROM EMPLOYEES
WHERE DEPARTMENT_ID =
	( SELECT DEPARTMENT_ID FROM DEPARTMENTS
	WHERE DEPARTMENT_NAME ='SHIPPING');
	
```

## 05: What do you mean by correlated subquery?
Correlated Subqueries are used to select data from a table referenced in the outer query.Correlated subquery references a column in the outer query and executes the subquery once for each row in the outer query and 'EXIST' is used in where clause.

```
SELECT * FROM EMPLOYEES E
WHERE EXISTS (SELECT * FROM DEPARTMENT D 
WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID 
AND D.NAME ='RESEARCH');

```

## 06: What do you mean by store procedure and where do you use it?

## 07: What do you mean by unique key and where do you use it?

## 08: What do you mean by primary key and where do you use it?


## 09: What do you mean by foreign key and where do you use it?

## 10: What is index in a data table and what is it's importance?



