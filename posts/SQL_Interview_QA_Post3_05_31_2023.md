# SQL Interview Questions and Answers Part 3

This is a collection of SQL interview question answers. Most of the questions are commonly asked in the SQL Developer/Programmer/Analyst position interview. Every post has covered 10 related questions and answers.

Different types of joins in SQL:

## 01: What are the different types of joins used in SQL:
There are six types of commonly used joins:

1. Inner Join
2. Left outer Join
3. Right outer Join
4. Full outer Join
5. Self Join
6. Cross Join

## 02: What do you mean by inner join and where do you use in SQL?
We we nned to join two or more than two tables horizontally then we use join. Inner join results a output table which contains only common records from two or more than two tables based on column selection.

Sample query:

```
SELECT E.EMPLOYEE_ID, E.FIRST_NAME, E.LAST_NAME, E.EMAIL, D.DEPARTMENT_ID, D.DEPARTMENT_NAME
FROM EMPLOYEES E
INNER JOIN DEPARTMENTS D
ON E.DEPARTMENT_ID=D.DEPARTMENT_ID
WHERE DEPARTMENT_NAME='IT';

```
Aove query results all employees id, name, and email who work for IT department.
Output result:

```
EMPLOYEE_ID FIRST_NAME LAST_NAME EMAIL DEPARTMENT_ID DEPARTMENT_NAME
103			Alexander	Hunold		AHUNOLD		60	IT
104			Bruce		Ernst		BERNST		60	IT
105			David		Austin		DAUSTIN		60	IT
106			Valli		Pataballa	VPATABAL	60	IT
107			Diana		Lorentz		DLORENTZ	60	IT

```
## 03: What do you mean by left outer join and where do you use in SQL?

Left outer join gives the output of all records from left table and matching records from the right table.

Sample query:

```
SELECT E.EMPLOYEE_ID, E.FIRST_NAME, E.LAST_NAME, E.EMAIL, D.DEPARTMENT_ID, D.DEPARTMENT_NAME
FROM EMPLOYEES E
left outer join departments d
on e.department_id=d.department_id

Note: left outer join and left join both result same output.

```

## 04: What do you mean by Right outer join and where do you use in SQL?

Right outer join gives the output of all records from right table and matching records from the left table.

Sample query:

```
SELECT E.EMPLOYEE_ID, E.FIRST_NAME, E.LAST_NAME, E.EMAIL, D.DEPARTMENT_ID, D.DEPARTMENT_NAME
FROM EMPLOYEES E
right outer join departments d
on e.department_id=d.department_id

Note: Right outer join and right join both result same output.

```
## 05: What do you mean by Full outer join and where do you use in SQL?

Full outer join gives the output of all records from left table and non matching records from the  table.

Sample query:

```
SELECT E.EMPLOYEE_ID, E.FIRST_NAME, E.LAST_NAME, E.EMAIL, D.DEPARTMENT_ID, D.DEPARTMENT_NAME
FROM EMPLOYEES E
FULL OUTER JOIN DEPARTMENTS D
ON E.DEPARTMENT_ID=D.DEPARTMENT_ID

```
## 06: What do you mean by self join and where do you use in SQL?
If a table is joined with same table then the type of operation is called self join. Self join is useful when we want to compare records within the same table or establish relationships between different rows in the same table.
Example: Employees table is given and you are asked to identify who will report to whom.

Sample query:

```
SELECT E.EMPLOYEE_ID
,E.FIRST_NAME AS EMPLOYEE_FNAME
,M.FIRST_NAME AS MANAGER_FNAME
FROM EMPLOYEES E
JOIN EMPLOYEES M
ON E.MANAGER_ID=M.EMPLOYEE_ID

```
Output result: Only 10 records are copied here.

```
EMPLOYEE_ID EMPLOYEE_FNAME MANAGER_FNAME
168				Lisa			Gerald
169				Harrison		Gerald
170				Tayler			Gerald
171				William			Gerald
172				Elizabeth		Gerald
173				Sundita			Gerald
103				Alexander		Lex
162				Clara			Alberto
163				Danielle		Alberto
164				Mattea			Alberto
```
## 07: How do you calculate how many people are reporting a manager?

Referring employees table. Create a table which has employee_id, Employee_Name, and Manager_Name then use group by with Manager_Name.

```
CREATE TABLE EMPLOYEE_MANAGER AS
SELECT E.EMPLOYEE_ID
,E.FIRST_NAME AS EMPLOYEE_FNAME
,M.FIRST_NAME AS MANAGER_FNAME
FROM EMPLOYEES E
JOIN EMPLOYEES M
ON E.MANAGER_ID=M.EMPLOYEE_ID

SELECT MANAGER_FNAME, COUNT(*) AS REPORTING_EMPLOYEE
FROM EMPLOYEE_MANAGER
GROUP BY MANAGER_FNAME;

```
Output result: Only 10 records are copied here.

```
MANAGER_FNAME, REPORTING_EMPLOYEE
David				3
Shelli				1
Amit				1
Lex					1
Adam				1
Renske				1
Jack				1
Eleni				1
Ki					1
Shelley				1

```
## 08: What do you mean by cross join and where do you use in SQL?
The CROSS JOIN in SQL, also known as a cartesian join, returns all combinations of rows from each table( If table1 has 5 rows and table2 has 4 rows then after cross join there will be 20 records in the final table.)
Cross join is not prefer for table having large number or records. It is ok for tables having less number or records and if we need to find the all combinations.


## 09: How does  ROW_NUMBER analytical function work?

ROW_NUMBER is a window function which is used to display a sequential number for each row in the report. This can help sort by criteria that don’t appear in the information or for debugging complex statements.

Sample query:

```
select Employee_id,
First_Name as Employee_Name,
Salary,
row_number() over (order by salary desc) as Row_Num
from employees;

```
Output result: Only 10 records are copied here.

```
Employee_id Employee_Name Salary Row_Num
100			Steven			24000	1
101			Neena			17000	2
102			Lex				17000	3
145			John			14000	4
146			Karen			13500	5
201			Michael			13000	6
108			Nancy			12000	7
147			Alberto			12000	8
205			Shelley			12000	9
168			Lisa			11500	10

```
## 10: What do you mean by Rank() function?

RANK(): It is a analytical or window function which provides the rank of the observations based on value. For two same value it provides same rank and the subsequent rank is skipped, resulting in non-consecutive ranks.

Sample query:

```
select Employee_id,
First_Name as Employee_Name,
Salary,
rank() over (order by salary desc) as Rank_Num
from employees;

```
Output result: Only 10 records are copied here.

```
Employee_id Employee_Name Salary Row_Num
100			Steven			24000	1
101			Neena			17000	2
102			Lex				17000	2
145			John			14000	4
146			Karen			13500	5
201			Michael			13000	6
108			Nancy			12000	7
147			Alberto			12000	7
205			Shelley			12000	7
168			Lisa			11500	10

```




